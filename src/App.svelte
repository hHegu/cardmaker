<script>
  import { onMount } from "svelte";
  import _ from "lodash";
  import firebase from "firebase";
  import uuidv1 from "uuid/v1";
  import Lander from "./pages/lander.svelte";
  import CardEditor from "./pages/card-editor.svelte";
  import CardList from "./pages/card-list.svelte";
  import firebaseConfig from "./config/firebase-config.json";

  let currentCard;
  let page = "card-editor";
  let cards = [];

  onMount(async () => {
    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);
    firebase
      .auth()
      .signInAnonymously()
      .catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
      });

    const addEmptyArraysForCards = cards => {
      _.forEach(cards, card => {
        card.abilities = card.abilities || [];
        card.actionCards = card.actionCards || [];
      });
    };

    loadCardsFromFirebase().then(loadedCards => {
      cards = _.toArray(loadedCards);
      console.log(loadedCards);
      addEmptyArraysForCards(cards);
      cards = cards;
      console.log(cards);
    });
  });

  const loadCardsFromFirebase = async () => {
    return firebase
      .database()
      .ref("cards/")
      .once("value")
      .then(snapshot => snapshot.val() || []);
  };

  const saveCardToFirebase = card =>
    firebase
      .database()
      .ref("cards/" + card.id)
      .set(card);

  const removeCardFromFirebase = ({ id }) =>
    firebase
      .database()
      .ref("cards/" + id)
      .remove();

  const saveImageToFirebase = (card, imageFile) => {
    // Create a root reference
    var storageRef = firebase.storage().ref();

    // Create a reference to 'images/mountains.jpg'
    var mountainImagesRef = storageRef.child(`images/${card.id}.jpg`);
    storageRef.put(imageFile)
  };

  const saveCard = ({ detail }) => {
    const { id } = detail;
    const existingCardId = _.findIndex(cards, card => id === card.id);
    if (existingCardId >= 0) {
      cards[existingCardId] = detail;
    } else {
      cards.push(_.cloneDeep(detail));
    }

    cards = cards;
    console.log(detail);
    saveCardToFirebase(detail);
    closeCardEditor();
  };

  const newCard = () => {
    currentCard = {
      id: uuidv1(),
      cost: null,
      name: null,
      type: "creature",
      backgroundImage: null,
      xp: 0,
      gold: 0,
      abilities: [],
      actionCards: {
        attacks: 0,
        defends: 0,
        specials: 0
      }
    };
  };

  const closeCardEditor = () => {
    currentCard = undefined;
  };

  const editCard = ({ detail }) => {
    currentCard = _.cloneDeep(detail);
  };

  const deleteCard = ({ detail }) => {
    cards.splice(_.findIndex(cards, card => card.id === detail.id), 1);
    removeCardFromFirebase(detail);
    cards = cards;
  };

  const copyCard = ({ detail }) => {
    const existingIndex = _.findIndex(cards, card => card.id === detail.id);
    const newCard = _.cloneDeep(detail);
    newCard.id = uuidv1();
    saveCardToFirebase(newCard);
    cards.splice(existingIndex, 0, newCard);
    cards = cards;
  };
</script>

<style>
  main {
    font-family: Roboto;
    height: 100%;
  }
  .cardmaker {
    display: flex;
    height: 100%;
  }
</style>

<main>
  {#if page === 'card-editor'}
    <div class="cardmaker">
      <CardEditor
        card={currentCard}
        on:saveImage={saveImageToFirebase}
        on:cancel={closeCardEditor}
        on:new={newCard}
        on:save={card => {
          saveCard(card);
          page = 'card-editor';
        }} />
      <CardList
        {cards}
        on:back={() => (page = 'lander')}
        on:edit={editCard}
        on:copy={copyCard}
        on:delete={deleteCard} />
    </div>
  {:else}
    <Lander
      on:card-editor={newCard}
      on:card-list={() => (page = 'card-editor')} />
  {/if}
</main>

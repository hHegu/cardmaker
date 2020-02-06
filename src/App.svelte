<script>
  import { onMount } from "svelte";
  import _ from "lodash";
  import firebase from "firebase";
  import uuidv1 from "uuid/v1";
  import Icon from "svelte-awesome";
  import { faSpinner } from "@fortawesome/free-solid-svg-icons/faSpinner";

  import Lander from "./pages/lander.svelte";
  import CardEditor from "./pages/card-editor.svelte";
  import CardList from "./pages/card-list.svelte";
  import Notifications from "./components/notifications.svelte";
  import firebaseConfig from "./config/firebase-config.json";

  let currentCard;
  let page = "card-editor";
  let cards = [];
  let loading = true;
  let notifications = [];

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

    loadCardsFromFirebase()
      .then(loadedCards => {
        cards = _.toArray(loadedCards);
        addEmptyArraysForCards(cards);
        cards = cards;
        loading = false;
      })
      .catch(e => showNotification({ message: e.message, isError: true }));
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
      .set(card, e =>
        showNotification({
          message: e ? e.message : "Save succesful",
          isError: e
        })
      );

  const removeCardFromFirebase = ({ id }) =>
    firebase
      .database()
      .ref("cards/" + id)
      .remove(e => showNotification({
          message: e ? e.message : "Remove succesful",
          isError: e
        }));

  const saveImageToFirebase = (card, imageFile) => {
    // Create a root reference
    var storageRef = firebase.storage().ref();

    // Create a reference to 'images/mountains.jpg'
    var mountainImagesRef = storageRef.child(`images/${card.id}.jpg`);
    storageRef.put(imageFile);
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

  const showNotification = notification => {
    const id = uuidv1();
    notifications.push({ id, ...notification });
    notifications = notifications;
    setTimeout(() => {
      notifications = _.filter(notifications, no => no.id !== id);
    }, 3000);
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

  .spinner {
    display: flex;
    flex: 1;
    align-items: center;
    justify-content: center;
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
      {#if loading}
        <div class="spinner">
          <Icon data={faSpinner} spin scale="3" />
        </div>
      {:else}
        <CardList
          {cards}
          on:back={() => (page = 'lander')}
          on:edit={editCard}
          on:copy={copyCard}
          on:delete={deleteCard} />
      {/if}

    </div>
  {:else}
    <Lander
      on:card-editor={newCard}
      on:card-list={() => (page = 'card-editor')} />
  {/if}
  <button on:click={() => showNotification({ message: 'Tere' })}>joo</button>
  <Notifications {notifications} />
</main>

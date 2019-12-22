<script>
  export let color = "black";
  export let size = "15";
</script>

<svg
  version="1.1"
  xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink="http://www.w3.org/1999/xlink"
  preserveAspectRatio="xMidYMid meet"
  viewBox="0 0 400 400"
  width={size}
  height={size}>
  <defs>
    <path
      d="M243.56 77.09C247.12 77.09 250 79.97 250 83.53C250 124.41 250 240.59
      250 281.47C250 285.03 247.12 287.91 243.56 287.91C214.85 287.91 135.15
      287.91 106.44 287.91C102.88 287.91 100 285.03 100 281.47C100 240.59 100
      124.41 100 83.53C100 79.97 102.88 77.09 106.44 77.09C135.15 77.09 214.85
      77.09 243.56 77.09Z"
      id="c1su6lRyvu" />
    <mask
      id="maskbqCzxXtOA"
      x="76"
      y="53.09"
      width="198"
      height="258.82"
      maskUnits="userSpaceOnUse">
      <rect x="76" y="53.09" width="198" height="258.82" fill="white" />
      <use xlink:href="#c1su6lRyvu" opacity="1" fill="black" />
    </mask>
    <path
      d="M293.56 112.09C297.12 112.09 300 114.97 300 118.53C300 159.41 300
      275.59 300 316.47C300 320.03 297.12 322.91 293.56 322.91C264.85 322.91
      185.15 322.91 156.44 322.91C152.88 322.91 150 320.03 150 316.47C150 275.59
      150 159.41 150 118.53C150 114.97 152.88 112.09 156.44 112.09C185.15 112.09
      264.85 112.09 293.56 112.09Z"
      id="c1KCKTyU5v" />
    <mask
      id="maska4buLWMs"
      x="126"
      y="88.09"
      width="198"
      height="258.82"
      maskUnits="userSpaceOnUse">
      <rect x="126" y="88.09" width="198" height="258.82" fill="white" />
      <use xlink:href="#c1KCKTyU5v" opacity="1" fill="black" />
    </mask>
  </defs>
  <g>
    <g>
      <g>
        <use
          xlink:href="#c1su6lRyvu"
          opacity="1"
          fill={color}
          fill-opacity="1" />
        <g mask="url(#maskbqCzxXtOA)">
          <use
            xlink:href="#c1su6lRyvu"
            opacity="1"
            fill-opacity="0"
            stroke="black"
            stroke-width="24"
            stroke-opacity="1" />
        </g>
      </g>
      <g>
        <use
          xlink:href="#c1KCKTyU5v"
          opacity="1"
          fill={color}
          fill-opacity="1" />
        <g mask="url(#maska4buLWMs)">
          <use
            xlink:href="#c1KCKTyU5v"
            opacity="1"
            fill-opacity="0"
            stroke="black"
            stroke-width="24"
            stroke-opacity="1" />
        </g>
      </g>
    </g>
  </g>
</svg>

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
      d="M268.56 95C272.12 95 275 97.88 275 101.44C275 142.32 275 258.5 275
      299.38C275 302.94 272.12 305.82 268.56 305.82C239.85 305.82 160.15 305.82
      131.44 305.82C127.88 305.82 125 302.94 125 299.38C125 258.5 125 142.32 125
      101.44C125 97.88 127.88 95 131.44 95C160.15 95 239.85 95 268.56 95Z"
      id="d2i3euKLpI" />
    <mask
      id="maskbhUKwhJvv"
      x="101"
      y="71"
      width="198"
      height="258.82"
      maskUnits="userSpaceOnUse">
      <rect x="101" y="71" width="198" height="258.82" fill="white" />
      <use xlink:href="#d2i3euKLpI" opacity="1" fill="black" />
    </mask>
  </defs>
  <g>
    <g>
      <g>
        <use
          xlink:href="#d2i3euKLpI"
          opacity="1"
          fill={color}
          fill-opacity="1" />
        <g mask="url(#maskbhUKwhJvv)">
          <use
            xlink:href="#d2i3euKLpI"
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

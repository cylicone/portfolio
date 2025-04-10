<script>
  // @ts-nocheck
  import { MapPin } from 'lucide-svelte';
  import { onMount } from 'svelte';
  import { fade, fly, scale, slide } from 'svelte/transition';
  import { backOut, elasticOut } from 'svelte/easing';
  
  // SVG components
  const TypeScriptLogo = `<svg width="24" height="24" viewBox="0 0 225 225" fill="none" xmlns="http://www.w3.org/2000/svg">
    <g clip-path="url(#clip0_19_18)">
      <path d="M0 112.5V225H225V0H0V112.5ZM181.308 103.528C187.024 104.955 191.377 107.522 195.405 111.656C197.459 113.85 200.531 117.879 200.791 118.87C200.862 119.166 191.088 125.719 185.154 129.382C184.936 129.53 184.057 128.616 183.101 127.181C180.211 122.97 177.166 121.141 172.519 120.811C165.705 120.368 161.311 123.926 161.346 129.895C161.311 131.358 161.641 132.827 162.295 134.107C163.8 137.222 166.62 139.085 175.331 142.861C191.447 149.78 198.33 154.357 202.612 160.84C207.408 168.089 208.47 179.663 205.249 188.304C201.663 197.641 192.797 204.012 180.345 206.1C176.498 206.796 167.344 206.684 163.202 205.917C154.16 204.307 145.589 199.835 140.316 193.978C138.227 191.707 134.198 185.737 134.459 185.337C134.571 185.189 135.485 184.605 136.512 184.015C137.538 183.424 141.307 181.23 144.865 179.177L151.348 175.409L152.705 177.42C154.61 180.309 158.745 184.268 161.241 185.583C168.42 189.394 178.27 188.845 183.143 184.486C185.119 182.841 186.223 180.309 186.075 177.75C186.075 175.148 185.745 173.981 184.388 172.034C182.63 169.545 179.079 167.421 168.933 163.027C157.324 158.006 152.304 154.934 147.769 149.991C144.914 146.728 142.791 142.889 141.581 138.748C140.737 135.562 140.52 127.575 141.18 124.39C143.564 113.147 152.058 105.349 164.25 103.043C168.209 102.277 177.434 102.565 181.315 103.521L181.308 103.528ZM128.503 112.943L128.573 122.133H99.2742V205.376H78.5461V122.133H49.2891V113.126C49.2891 108.105 49.4016 103.936 49.5422 103.823C49.6547 103.676 67.4508 103.605 89.0578 103.641L128.391 103.753L128.503 112.943Z" fill="#3178C6"/>
    </g>
    <defs>
      <clipPath id="clip0_19_18">
        <rect width="225" height="225" fill="white"/>
      </clipPath>
    </defs>
  </svg>`;
  
  const GitHubLogo = `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-full h-full">
    <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"></path>
  </svg>`;
  
  const TailwindLogo = `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="w-full h-full">
    <path fill="#06B6D4" d="M12.001,4.8c-3.2,0-5.2,1.6-6,4.8c1.2-1.6,2.6-2.2,4.2-1.8c0.913,0.228,1.565,0.89,2.288,1.624 C13.666,10.618,15.027,12,18.001,12c3.2,0,5.2-1.6,6-4.8c-1.2,1.6-2.6,2.2-4.2,1.8c-0.913-0.228-1.565-0.89-2.288-1.624 C16.337,6.182,14.976,4.8,12.001,4.8z M6.001,12c-3.2,0-5.2,1.6-6,4.8c1.2-1.6,2.6-2.2,4.2-1.8c0.913,0.228,1.565,0.89,2.288,1.624 c1.177,1.194,2.538,2.576,5.512,2.576c3.2,0,5.2-1.6,6-4.8c-1.2,1.6-2.6,2.2-4.2,1.8c-0.913-0.228-1.565-0.89-2.288-1.624 C10.337,13.382,8.976,12,6.001,12z"/>
  </svg>`;
  
  const VercelLogo = `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="w-full h-full">
    <path fill="white" d="M12 2L2 19.7778H22L12 2Z"/>
  </svg>`;
  
  const ReactLogo = `<svg xmlns="http://www.w3.org/2000/svg" viewBox="-11.5 -10.23174 23 20.46348" class="w-full h-full">
    <title>React Logo</title>
    <circle cx="0" cy="0" r="2.05" fill="#61dafb"/>
    <g stroke="#61dafb" stroke-width="1" fill="none">
      <ellipse rx="11" ry="4.2"/>
      <ellipse rx="11" ry="4.2" transform="rotate(60)"/>
      <ellipse rx="11" ry="4.2" transform="rotate(120)"/>
    </g>
  </svg>`;
  
  const GitLogo = `<svg width="24" height="24" viewBox="0 0 512 512" fill="none" xmlns="http://www.w3.org/2000/svg">
    <g clip-path="url(#clip0_21_3)">
      <path d="M501.862 279.793L279.792 501.858C266.93 514.708 246.098 514.708 233.252 501.858L10.141 278.762C-2.713 265.911 -2.713 245.072 10.141 232.209L163.121 79.2351L221.054 137.177C215.114 151.406 217.902 168.41 229.49 179.988C233.277 183.787 237.643 186.623 242.309 188.532V328.83C237.643 330.743 233.27 333.596 229.49 337.372C214.193 352.667 214.193 377.443 229.49 392.755C244.771 408.051 269.564 408.051 284.863 392.755C300.158 377.443 300.158 352.667 284.863 337.372C281.753 334.278 278.234 331.816 274.505 329.97L274.509 190.96L327.335 243.786C321.509 257.96 324.353 274.86 335.856 286.371C351.164 301.675 375.944 301.675 391.248 286.371C406.543 271.072 406.543 246.292 391.248 230.999C380.303 220.037 364.492 216.984 350.79 221.706L294.15 165.067C298.879 151.36 295.805 135.566 284.864 124.613C273.977 113.725 258.318 110.632 244.658 115.244L185.891 56.4751L232.217 10.1421C245.073 -2.70995 265.913 -2.70995 278.75 10.1421L501.862 233.24C514.712 246.092 514.712 266.938 501.862 279.793Z" fill="#F1502F"/>
    </g>
    <defs>
      <clipPath id="clip0_21_3">
        <rect width="512" height="512" fill="white"/>
      </clipPath>
    </defs>
  </svg>`;
  
  const SupabaseLogo = `<svg width="24" height="24" viewBox="0 0 109 113" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path d="M63.7076 110.284C60.8481 113.885 55.0502 111.912 54.9813 107.314L53.9738 40.0627L99.1935 40.0627C107.384 40.0627 111.952 49.5228 106.859 55.9374L63.7076 110.284Z" fill="url(#paint0_linear)"/>
    <path d="M63.7076 110.284C60.8481 113.885 55.0502 111.912 54.9813 107.314L53.9738 40.0627L99.1935 40.0627C107.384 40.0627 111.952 49.5228 106.859 55.9374L63.7076 110.284Z" fill="url(#paint1_linear)" fill-opacity="0.2"/>
    <path d="M45.317 2.07103C48.1765 -1.53037 53.9745 0.442937 54.0434 5.041L54.4849 72.2922H9.83113C1.64038 72.2922 -2.92775 62.8321 2.1655 56.4175L45.317 2.07103Z" fill="#3ECF8E"/>
    <defs>
      <linearGradient id="paint0_linear" x1="53.9738" y1="54.974" x2="94.1635" y2="71.8295" gradientUnits="userSpaceOnUse">
        <stop stop-color="#249361"/>
        <stop offset="1" stop-color="#3ECF8E"/>
      </linearGradient>
      <linearGradient id="paint1_linear" x1="36.1558" y1="30.578" x2="54.4844" y2="65.0806" gradientUnits="userSpaceOnUse">
        <stop/>
        <stop offset="1" stop-opacity="0"/>
      </linearGradient>
    </defs>
  </svg>`;
  
  const PostgreSQLLogo = `<svg version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 118.53 122.88" class="w-full h-full">
    <style type="text/css">
      .st0{fill:#FFFFFF;}
      .st1{fill:#336791;}
    </style>
    <g>
      <path class="st1" d="M113.53,75.55c-14.65,3.02-15.66-1.94-15.66-1.94c15.47-22.95,21.93-52.09,16.35-59.22 C99-5.06,72.65,4.14,72.21,4.38L72.06,4.4c-2.89-0.6-6.13-0.96-9.77-1.02c-6.63-0.11-11.66,1.74-15.47,4.63 c0,0-47.01-19.37-44.82,24.36c0.47,9.3,13.33,70.38,28.68,51.93c5.61-6.75,11.03-12.45,11.03-12.45c2.69,1.79,5.92,2.7,9.29,2.37 L51.26,74c-0.08,0.84-0.04,1.66,0.1,2.63c-3.95,4.42-2.79,5.19-10.7,6.82c-8,1.65-3.3,4.58-0.23,5.35 c3.72,0.93,12.32,2.25,18.14-5.89l-0.23,0.93c1.55,1.24,2.64,8.07,2.46,14.26c-0.18,6.19-0.3,10.44,0.92,13.76 s2.44,10.79,12.83,8.56c8.68-1.86,13.18-6.68,13.81-14.72c0.44-5.72,1.45-4.87,1.51-9.98l0.81-2.42c0.93-7.75,0.15-10.25,5.5-9.09 l1.3,0.11c3.94,0.18,9.09-0.63,12.11-2.04C116.08,79.27,119.95,74.22,113.53,75.55L113.53,75.55L113.53,75.55z"/>
      <path class="st0" d="M57.39,79.94c0.03-1,0.86-1.79,1.86-1.76s1.79,0.86,1.76,1.86c-0.21,7.43-0.17,14.88,0.1,20.84 c0.25,5.37,0.68,9.41,1.28,10.91c0.64,1.59,1.63,3.94,3.53,5.53c1.85,1.56,4.7,2.51,9.24,1.54c3.97-0.85,6.64-2.06,8.42-3.91 c1.76-1.83,2.76-4.42,3.38-8.04c0.46-2.64,1.09-7.25,1.66-11.84c0.72-5.74,1.38-11.54,1.52-13.41c0.07-1,0.94-1.75,1.94-1.67 c1,0.07,1.75,0.94,1.67,1.94c-0.14,1.82-0.8,7.69-1.55,13.6c-0.6,4.74-1.24,9.44-1.68,11.99c-0.75,4.33-2.02,7.52-4.35,9.94 c-2.32,2.41-5.57,3.93-10.28,4.94c-5.85,1.25-9.71-0.12-12.32-2.32c-2.57-2.16-3.79-5.02-4.56-6.95c-0.76-1.89-1.27-6.35-1.54-12.1 C57.21,95.02,57.18,87.47,57.39,79.94L57.39,79.94z"/>
      <path class="st0" d="M47.41,6.01c0.93,0.37,1.38,1.42,1.01,2.35c-0.37,0.93-1.42,1.38-2.35,1.01C45.95,9.32,1.65-8.78,3.7,32.1 c0.25,5.05,4.25,25.71,10.33,40.12c1.92,4.54,4.03,8.42,6.27,10.9c1.87,2.08,3.78,3.08,5.62,2.42c1.02-0.37,2.11-1.2,3.26-2.58 c5.63-6.77,10.75-12.12,10.77-12.13c0.69-0.72,1.83-0.75,2.55-0.06c0.72,0.69,0.75,1.83,0.06,2.55c-0.01,0.01-5.01,5.22-10.6,11.95 c-1.59,1.92-3.21,3.11-4.84,3.69c-3.46,1.23-6.64-0.21-9.52-3.41c-2.52-2.79-4.84-7.02-6.9-11.93C4.45,58.83,0.34,37.51,0.08,32.27 C-2.25-14.29,47.27,5.95,47.41,6.01L47.41,6.01z"/>
      <path class="st0" d="M73.1,5.73l-2.69-2.94c0.22-0.08,0.44-0.15,0.66-0.21c0.59-0.19,1.23-0.38,1.92-0.58 c1.01-0.3,2.49-0.69,4.3-1.04c9.03-1.75,26.65-2.7,38.26,12.14c3.28,4.19,3.01,14.56-0.38,26.84c-3,10.87-8.41,23.4-15.89,34.5 c-0.56,0.83-1.69,1.05-2.52,0.49s-1.05-1.69-0.49-2.52c7.26-10.77,12.5-22.92,15.41-33.44c3.1-11.22,3.59-20.37,1.02-23.65 C102.28,2.01,86.21,2.91,77.96,4.52C76.27,4.85,74.92,5.2,74,5.47L73.1,5.73L73.1,5.73z"/>
      <path class="st0" d="M99.49,72.87c0.02,0.05,0.03,0.1,0.04,0.15c0,0.01,0.62,3.24,13.53,0.58c1.83-0.38,3.13-0.3,3.97,0.11 c1.09,0.52,1.56,1.39,1.49,2.51c-0.04,0.68-0.36,1.41-0.89,2.14c-1.22,1.67-4.01,3.83-7.4,5.4c-2.54,1.18-6.41,1.97-9.96,2.17 c-2.09,0.12-4.09,0.04-5.68-0.28c-2-0.4-3.52-1.23-4.15-2.58c-0.17-0.36-0.27-0.75-0.29-1.17c-0.4-7.02,3.02-8.25,5.41-8.93 c-0.25-0.36-0.59-0.79-0.97-1.26c-1.21-1.51-2.76-3.45-3.99-6.42c-0.19-0.45-0.74-1.45-1.48-2.77c-3.13-5.61-9.29-16.67-8.08-24.22 c0.86-5.39,4.97-8.97,15.24-7.9l-0.1-0.3c-0.49-1.39-1.27-3.21-2.4-5.25c-4.48-8.16-14.25-19.54-31.6-19.82 c-26.47-0.43-25.6,32.95-25.6,33.04c0.02,1-0.78,1.82-1.78,1.84s-1.82-0.78-1.84-1.78c0-0.11-0.98-37.21,29.27-36.72 c19.1,0.31,29.82,12.77,34.72,21.69c1.22,2.22,2.09,4.24,2.64,5.81c0.62,1.76,0.85,3.14,0.75,3.75c-0.14,0.9-0.64,1.46-1.5,1.63 l-0.65,0.01c-9.42-1.51-12.98,0.8-13.58,4.56c-1.01,6.33,4.75,16.65,7.67,21.9c0.79,1.42,1.39,2.5,1.66,3.16 c1.04,2.51,2.4,4.21,3.46,5.55C98.39,70.68,99.17,71.66,99.49,72.87L99.49,72.87z M113.79,77.14c-9.57,1.98-14.03,0.53-16.08-1.03 c-0.28,0.09-0.58,0.17-0.9,0.26c-1.28,0.36-3.27,0.92-3.06,5.22c0.15,0.16,0.71,0.33,1.53,0.5c1.31,0.26,2.99,0.32,4.78,0.22 c3.13-0.18,6.5-0.85,8.65-1.85c2.21-1.03,4.06-2.27,5.2-3.35L113.79,77.14L113.79,77.14z"/>
      <path class="st0" d="M52.6,77.67c-0.74,0.83-1.35,1.6-1.87,2.26c-2.22,2.82-2.91,3.7-9.8,5.12c-1.24,0.26-2.08,0.51-2.6,0.76 c0.64,0.46,1.6,0.84,2.44,1.05c1.7,0.43,4.52,0.94,7.54,0.43c2.94-0.5,6.11-2.01,8.68-5.61c0.08-0.12,0.15-0.25,0.21-0.38 c0.26-0.63,0.2-1.42-0.06-2.17c-0.28-0.8-0.79-1.5-1.38-1.93c-0.14-0.1-0.28-0.18-0.43-0.24l-0.09-0.04 c-0.37-0.15-0.78-0.31-1.13-0.28C53.73,76.68,53.25,76.94,52.6,77.67L52.6,77.67L52.6,77.67z M47.88,77.68 c0.49-0.62,1.06-1.34,1.92-2.31c0.07-0.09,0.15-0.18,0.23-0.26c1.33-1.44,2.6-1.98,3.77-2.08c1.16-0.1,2.06,0.23,2.82,0.55 c0.03,0.01,0.06,0.02,0.1,0.04c0.39,0.16,0.77,0.38,1.14,0.64c1.19,0.85,2.17,2.2,2.7,3.67c0.54,1.53,0.63,3.24-0.01,4.76 c-0.16,0.38-0.36,0.74-0.61,1.09c-3.24,4.54-7.28,6.44-11.02,7.08c-3.67,0.63-7.01,0.02-9.03-0.48c-1.47-0.37-3.29-1.19-4.36-2.19 c-0.63-0.59-1.06-1.29-1.18-2.07c-0.14-0.93,0.14-1.83,0.99-2.63c0.82-0.77,2.34-1.47,4.85-1.99 C45.78,80.35,46.28,79.72,47.88,77.68L47.88,77.68z"/>
      <path class="st0" d="M52.79,76.1c0.15,0.99-0.53,1.92-1.52,2.06s-1.92-0.53-2.06-1.52c-0.04-0.24-0.06-0.48-0.08-0.73 c-0.6-0.02-1.19-0.06-1.77-0.15c-3.88-0.54-7.39-2.5-10.03-5.4c-2.61-2.86-4.37-6.64-4.78-10.86c-0.15-1.5-0.12-3.06,0.1-4.63 c0.89-6.34,0.55-12.01,0.38-14.95c-0.05-0.8-0.08-1.4-0.08-1.86c0.01-1.19,3-3.81,6.94-5.43c1.78-0.73,3.77-1.34,5.74-1.64 c2.08-0.31,4.18-0.28,6.07,0.31c2.96,0.93,5.31,3.12,6.21,7.31c3.25,15.01,0.34,21.46-1.93,26.51c-0.39,0.87-0.76,1.69-1.04,2.43 c-0.01,0.05-0.03,0.09-0.05,0.14c-0.95,2.56,0.44-1.19-0.28,0.72C53.48,71.49,52.49,74.13,52.79,76.1L52.79,76.1z M49.52,72.3 c0.39-1.61,1.03-3.31,1.71-5.14c0.54-1.44-0.87,2.33,0.26-0.72c0.02-0.05,0.04-0.1,0.06-0.15c0.35-0.93,0.73-1.76,1.12-2.64 c2.06-4.57,4.68-10.41,1.69-24.25c-0.58-2.69-1.99-4.07-3.75-4.62c-1.32-0.41-2.87-0.42-4.46-0.18c-1.7,0.26-3.4,0.78-4.92,1.4 c-2.67,1.09-4.69,2.01-4.69,2.09c0,0.26,0.03,0.85,0.08,1.64c0.18,3.05,0.53,8.96-0.4,15.66c-0.18,1.31-0.21,2.58-0.09,3.8 c0.33,3.4,1.75,6.45,3.86,8.76c2.08,2.28,4.83,3.83,7.85,4.25C48.4,72.26,48.96,72.3,49.52,72.3L49.52,72.3z"/>
      <path class="st0" d="M46.57,37.82c-0.08,0.59,1.09,2.18,2.61,2.39c1.52,0.21,2.83-1.03,2.91-1.62c0.08-0.59-1.09-1.25-2.61-1.46 C47.96,36.92,46.65,37.23,46.57,37.82L46.57,37.82L46.57,37.82z"/>
      <path class="st0" d="M47.17,37.9c-0.02,0.13,0.19,0.47,0.54,0.84c0.39,0.41,0.94,0.78,1.55,0.87l0.01,0 c0.6,0.08,1.16-0.12,1.58-0.41c0.39-0.26,0.63-0.54,0.66-0.7c0-0.03-0.15-0.12-0.4-0.26c-0.41-0.23-1.01-0.42-1.7-0.52l-0.02,0 c-0.68-0.09-1.31-0.07-1.76,0.04C47.35,37.83,47.17,37.88,47.17,37.9L47.17,37.9L47.17,37.9z M46.84,39.57 c-0.59-0.62-0.92-1.36-0.86-1.82l0-0.02c0.08-0.57,0.59-0.96,1.37-1.15c0.59-0.14,1.37-0.17,2.2-0.06c0.01,0,0.02,0,0.03,0 c0.84,0.12,1.59,0.36,2.12,0.66c0.7,0.39,1.09,0.92,1.01,1.48c-0.06,0.44-0.51,1.08-1.18,1.53c-0.63,0.43-1.49,0.74-2.42,0.61 l-0.01,0C48.18,40.68,47.39,40.15,46.84,39.57L46.84,39.57z"/>
      <path class="st0" d="M93.01,36.61c0.08,0.59-1.09,2.18-2.61,2.39c-1.53,0.21-2.83-1.03-2.91-1.62c-0.08-0.59,1.09-1.25,2.62-1.46 C91.62,35.71,92.92,36.02,93.01,36.61L93.01,36.61L93.01,36.61z"/>
      <path class="st0" d="M93.3,36.57L93.3,36.57c0.05,0.38-0.25,1.03-0.78,1.59c-0.51,0.54-1.25,1.03-2.09,1.14l-0.01,0 c-0.84,0.11-1.62-0.17-2.2-0.56c-0.6-0.4-0.99-0.95-1.05-1.32v0c-0.06-0.43,0.27-0.85,0.86-1.18c0.5-0.28,1.21-0.51,2.01-0.62 c0.8-0.11,1.55-0.08,2.11,0.05C92.81,35.82,93.24,36.13,93.3,36.57L93.3,36.57z M92.08,37.74c0.41-0.43,0.65-0.87,0.62-1.09 c-0.02-0.16-0.28-0.3-0.68-0.4h0c-0.49-0.12-1.16-0.14-1.89-0.04c-0.72,0.1-1.36,0.31-1.8,0.55c-0.36,0.2-0.57,0.41-0.55,0.57 c0.03,0.22,0.33,0.59,0.79,0.9c0.47,0.32,1.11,0.55,1.79,0.46l0.01,0C91.04,38.6,91.65,38.19,92.08,37.74L92.08,37.74z"/>
      <path class="st0" d="M96.65,32.59c-0.05-1,0.72-1.85,1.72-1.9c1-0.05,1.85,0.72,1.9,1.72c0.15,2.8-0.19,5.02-0.55,7.35 c-0.27,1.74-0.55,3.56-0.61,5.59c-0.06,1.93,0.19,4.07,0.46,6.27c0.7,5.79,1.44,11.99-2.86,18.4c-0.56,0.83-1.68,1.05-2.51,0.5 c-0.83-0.56-1.05-1.68-0.5-2.51c3.54-5.29,2.88-10.8,2.26-15.96c-0.28-2.3-0.55-4.54-0.47-6.8c0.07-2.34,0.36-4.21,0.64-6.02 C96.47,37.05,96.78,34.99,96.65,32.59L96.65,32.59z"/>
    </g>
  </svg>`;
  
  // Add the GoLang logo SVG
  const GoLangLogo = `<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 254.5 225" class="w-full h-full">
    <style type="text/css">
      .st0{fill:#2DBCAF;}
      .st1{fill:#5DC9E1;}
      .st2{fill:#FDDD00;}
      .st3{fill:#CE3262;}
      .st4{fill:#00ACD7;}
      .st5{fill:#FFFFFF;}
    </style>
    <g>
      <g>
        <g>
          <g>
            <path class="st4" d="M40.2,101.1c-0.4,0-0.5-0.2-0.3-0.5l2.1-2.7c0.2-0.3,0.7-0.5,1.1-0.5l35.7,0c0.4,0,0.5,0.3,0.3,0.6      l-1.7,2.6c-0.2,0.3-0.7,0.6-1,0.6L40.2,101.1z"/>
          </g>
        </g>
      </g>
      <g>
        <g>
          <g>
            <path class="st4" d="M25.1,110.3c-0.4,0-0.5-0.2-0.3-0.5l2.1-2.7c0.2-0.3,0.7-0.5,1.1-0.5l45.6,0c0.4,0,0.6,0.3,0.5,0.6      l-0.8,2.4c-0.1,0.4-0.5,0.6-0.9,0.6L25.1,110.3z"/>
          </g>
        </g>
      </g>
      <g>
        <g>
          <g>
            <path class="st4" d="M49.3,119.5c-0.4,0-0.5-0.3-0.3-0.6l1.4-2.5c0.2-0.3,0.6-0.6,1-0.6l20,0c0.4,0,0.6,0.3,0.6,0.7l-0.2,2.4      c0,0.4-0.4,0.7-0.7,0.7L49.3,119.5z"/>
          </g>
        </g>
      </g>
      <g>
        <g id="CXHf1q_3_">
          <g>
            <g>
              <path class="st4" d="M153.1,99.3c-6.3,1.6-10.6,2.8-16.8,4.4c-1.5,0.4-1.6,0.5-2.9-1c-1.5-1.7-2.6-2.8-4.7-3.8       c-6.3-3.1-12.4-2.2-18.1,1.5c-6.8,4.4-10.3,10.9-10.2,19c0.1,8,5.6,14.6,13.5,15.7c6.8,0.9,12.5-1.5,17-6.6       c0.9-1.1,1.7-2.3,2.7-3.7c-3.6,0-8.1,0-19.3,0c-2.1,0-2.6-1.3-1.9-3c1.3-3.1,3.7-8.3,5.1-10.9c0.3-0.6,1-1.6,2.5-1.6       c5.1,0,23.9,0,36.4,0c-0.2,2.7-0.2,5.4-0.6,8.1c-1.1,7.2-3.8,13.8-8.2,19.6c-7.2,9.5-16.6,15.4-28.5,17       c-9.8,1.3-18.9-0.6-26.9-6.6c-7.4-5.6-11.6-13-12.7-22.2c-1.3-10.9,1.9-20.7,8.5-29.3c7.1-9.3,16.5-15.2,28-17.3       c9.4-1.7,18.4-0.6,26.5,4.9c5.3,3.5,9.1,8.3,11.6,14.1C154.7,98.5,154.3,99,153.1,99.3z"/>
            </g>
            <g>
              <path class="st4" d="M186.2,154.6c-9.1-0.2-17.4-2.8-24.4-8.8c-5.9-5.1-9.6-11.6-10.8-19.3c-1.8-11.3,1.3-21.3,8.1-30.2       c7.3-9.6,16.1-14.6,28-16.7c10.2-1.8,19.8-0.8,28.5,5.1c7.9,5.4,12.8,12.7,14.1,22.3c1.7,13.5-2.2,24.5-11.5,33.9       c-6.6,6.7-14.7,10.9-24,12.8C191.5,154.2,188.8,154.3,186.2,154.6z M210,114.2c-0.1-1.3-0.1-2.3-0.3-3.3       c-1.8-9.9-10.9-15.5-20.4-13.3c-9.3,2.1-15.3,8-17.5,17.4c-1.8,7.8,2,15.7,9.2,18.9c5.5,2.4,11,2.1,16.3-0.6       C205.2,129.2,209.5,122.8,210,114.2z"/>
            </g>
          </g>
        </g>
      </g>
    </g>
  </svg>`;
  
  // Add the SvelteKit logo SVG
  const SvelteKitLogo = `<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 98.1 118" class="w-full h-full" style="enable-background:new 0 0 98.1 118;">
    <path fill="#FF3E00" d="M91.8,15.6C80.9-0.1,59.2-4.7,43.6,5.2L16.1,22.8C8.6,27.5,3.4,35.2,1.9,43.9c-1.3,7.3-0.2,14.8,3.3,21.3  c-2.4,3.6-4,7.6-4.7,11.8c-1.6,8.9,0.5,18.1,5.7,25.4c11,15.7,32.6,20.3,48.2,10.4l27.5-17.5c7.5-4.7,12.7-12.4,14.2-21.1  c1.3-7.3,0.2-14.8-3.3-21.3c2.4-3.6,4-7.6,4.7-11.8C99.2,32.1,97.1,22.9,91.8,15.6"/>
    <path fill="#FFFFFF" d="M40.9,103.9c-8.9,2.3-18.2-1.2-23.4-8.7c-3.2-4.4-4.4-9.9-3.5-15.3c0.2-0.9,0.4-1.7,0.6-2.6l0.5-1.6l1.4,1  c3.3,2.4,6.9,4.2,10.8,5.4l1,0.3l-0.1,1c-0.1,1.4,0.3,2.9,1.1,4.1c1.6,2.3,4.4,3.4,7.1,2.7c0.6-0.2,1.2-0.4,1.7-0.7L65.5,72  c1.4-0.9,2.3-2.2,2.6-3.8c0.3-1.6-0.1-3.3-1-4.6c-1.6-2.3-4.4-3.3-7.1-2.6c-0.6,0.2-1.2,0.4-1.7,0.7l-10.5,6.7  c-1.7,1.1-3.6,1.9-5.6,2.4c-8.9,2.3-18.2-1.2-23.4-8.7c-3.1-4.4-4.4-9.9-3.4-15.3c0.9-5.2,4.1-9.9,8.6-12.7l27.5-17.5  c1.7-1.1,3.6-1.9,5.6-2.5c8.9-2.3,18.2,1.2,23.4,8.7c3.2,4.4,4.4,9.9,3.5,15.3c-0.2,0.9-0.4,1.7-0.7,2.6l-0.5,1.6l-1.4-1  c-3.3-2.4-6.9-4.2-10.8-5.4l-1-0.3l0.1-1c0.1-1.4-0.3-2.9-1.1-4.1c-1.6-2.3-4.4-3.3-7.1-2.6c-0.6,0.2-1.2,0.4-1.7,0.7L32.4,46.1  c-1.4,0.9-2.3,2.2-2.6,3.8s0.1,3.3,1,4.6c1.6,2.3,4.4,3.3,7.1,2.6c0.6-0.2,1.2-0.4,1.7-0.7l10.5-6.7c1.7-1.1,3.6-1.9,5.6-2.5  c8.9-2.3,18.2,1.2,23.4,8.7c3.2,4.4,4.4,9.9,3.5,15.3c-0.9,5.2-4.1,9.9-8.6,12.7l-27.5,17.5C44.8,102.5,42.9,103.3,40.9,103.9"/>
  </svg>`;
  
  // Navigation links
  const navLinks = [
    { icon: 'github', url: 'https://github.com/clamared', label: 'GitHub', svg: GitHubLogo },
  ];

  // Current technologies
  const currentTechnologies = [
    { name: 'GoLang', description: 'Programming language', icon: GoLangLogo, color: 'rgba(0, 173, 216, 0.2)' },
    { name: 'TypeScript', description: 'JavaScript but less terrible', icon: TypeScriptLogo, color: 'rgba(49, 120, 198, 0.2)' },
    { name: 'React', description: 'JavaScript Library', icon: ReactLogo, color: 'rgba(97, 218, 251, 0.2)' },
    { name: 'SvelteKit', description: 'Svelte framework', icon: SvelteKitLogo, color: 'rgba(255, 62, 0, 0.2)' },
    { name: 'Tailwind', description: 'CSS framework', icon: TailwindLogo, color: 'rgba(6, 182, 212, 0.2)' },
    { name: 'Git', description: 'Version control', icon: GitLogo, color: 'rgba(240, 80, 50, 0.2)' },
    { name: 'Supabase', description: 'Backend tool', icon: SupabaseLogo, color: 'rgba(62, 207, 142, 0.2)' },
    { name: 'Postgres', description: 'Backend Database', icon: PostgreSQLLogo, color: 'rgba(240, 46, 101, 0.2)' }
  ];

  // Other technologies
  const otherTechnologies = [
    { name: 'Typescript', icon: 'typescript', svg: TypeScriptLogo },
    { name: 'GitHub', icon: 'github', svg: GitHubLogo },
    { name: 'GitHub Actions', icon: 'github-actions' },
    { name: 'AWS', icon: 'aws' },
    { name: 'Vercel', icon: 'vercel', svg: VercelLogo },
    { name: 'MySQL', icon: 'mysql' },
    { name: 'SQLite', icon: 'sqlite' },
    { name: 'Nginx', icon: 'nginx' }
  ];

  // Animation control variables
  let mounted = false;
  let scrollY;
  let innerHeight;
  let sections = [];
  let sectionElements = [];
  
  // Track scroll position for animations
  onMount(() => {
    setTimeout(() => {
      mounted = true;
    }, 100);
    
    // Get all section elements for scroll animations
    sectionElements = document.querySelectorAll('section');
    
    // Initialize sections array with visibility status
    sections = Array.from(sectionElements).map(() => false);
    
    // Check which sections are visible on scroll
    function checkVisibility() {
      sectionElements.forEach((section, index) => {
        const rect = section.getBoundingClientRect();
        const isVisible = (
          rect.top <= innerHeight * 0.8 && 
          rect.bottom >= innerHeight * 0.2
        );
        sections[index] = isVisible;
      });
      sections = [...sections]; // Trigger reactivity
    }
    
    // Initial check
    checkVisibility();
    
    // Add scroll event listener
    window.addEventListener('scroll', checkVisibility);
    
    return () => {
      window.removeEventListener('scroll', checkVisibility);
    };
  });
  
  // Function to stagger animations of children
  function staggerChildren(node, { delay = 0, duration = 300, easing = backOut }) {
    const children = Array.from(node.children);
    
    children.forEach((child, i) => {
      const childDelay = delay + (i * 100);
      
      child.style.opacity = '0';
      child.style.transform = 'translateY(20px)';
      
      setTimeout(() => {
        child.style.transition = `opacity ${duration}ms ${easing}, transform ${duration}ms ${easing}`;
        child.style.opacity = '1';
        child.style.transform = 'translateY(0)';
      }, childDelay);
    });
    
    return {
      destroy() {
        // Cleanup if needed
      }
    };
  }
</script>

<svelte:window bind:scrollY bind:innerHeight />

<div class="bg-black text-white min-h-screen relative">
  <!-- Background stars effect with parallax -->
  <div 
    class="stars-bg fixed inset-0 z-0" 
    style="transform: translateY({scrollY * 0.1}px)"
  ></div>
  
  <!-- Content container -->
  <div class="relative z-10 container max-w-5xl mx-auto px-4 py-6">
    <!-- Header with fade in animation -->
    <header 
      class="flex justify-between items-center mb-8 md:mb-12"
      in:fade={{ duration: 800, delay: 200 }}
    >
      <div class="text-xl font-bold">Ethan Annane</div>
      
      <div class="flex space-x-2 md:space-x-4">
        {#each navLinks as link, i}
          {#if mounted}
            <a 
              href={link.url} 
              class="p-2 md:p-3 bg-zinc-900 rounded-lg hover:bg-zinc-800 transition-all duration-300 hover:scale-110"
              in:fly={{ y: -20, duration: 400, delay: 300 + (i * 100) }}
            >
              {#if link.svg}
                <div class="w-5 h-5 md:w-6 md:h-6">
                  {@html link.svg}
                </div>
              {:else}
                <div class="w-5 h-5 md:w-6 md:h-6 icon-placeholder" data-icon="{link.icon}"></div>
              {/if}
            </a>
          {/if}
        {/each}
      </div>
    </header>
    
    <!-- Main intro section -->
    <section class="grid grid-cols-1 lg:grid-cols-4 gap-4 md:gap-6 mb-6 md:mb-8">
      {#if mounted}
        <div 
          class="col-span-1 lg:col-span-3 bg-zinc-900 rounded-lg p-4 md:p-6"
          in:fly={{ x: -50, duration: 800, delay: 400 }}
        >
          <h1 class="text-2xl md:text-3xl font-bold mb-4">
            Hello! <span class="wave-emoji">👋</span> My name is <span class="text-blue-500">Ethan Annane</span>.
          </h1>
          
          <div class="flex items-center mb-4">
            <div class="mr-2 w-4 h-4" data-icon="location"><MapPin size={16} /></div>
            <p class="text-gray-400 text-sm md:text-base"> Bunbury, Western Australia.</p>
          </div>
          
          <p class="mb-4 text-sm md:text-base">Hi! Im 13 and am a beginner full stack software developer.</p>
          <p class="text-gray-300 text-sm md:text-base">I enjoy playing around with whatever cool ideas come to my mind, and building open source tools for myself, and others to enjoy!</p>
        </div>
        
        <div 
          class="col-span-1 bg-zinc-900 rounded-lg overflow-hidden"
          in:scale={{ start: 0.8, duration: 800, delay: 600, easing: elasticOut }}
        >
          <!-- Profile photo -->
          <img 
            src="/me.png" 
            alt="Ethan Annane" 
            class="w-full h-full object-cover min-h-[200px] md:min-h-[300px]"
          />
        </div>
      {/if}
    </section>
    
    <!-- Current technologies section -->
    <section class="mb-6 md:mb-8">
      <div in:fade={{ duration: 400 }}>
        <h2 class="text-lg md:text-xl font-bold mb-2">Current technologies</h2>
        <p class="text-gray-400 text-xs md:text-sm mb-3 md:mb-4">I'm proficient in a range of modern technologies that empower me to build highly functional solutions. These are some of my main technologies.</p>
      </div>
      
      <div 
        class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-2 md:gap-3"
        use:staggerChildren={{ delay: 100 }}
      >
        {#each currentTechnologies as tech}
          <div class="bg-zinc-900 p-2 md:p-3 rounded-lg flex items-start hover:bg-zinc-800 transition-all duration-300 hover:scale-105">
            <div 
              class="mr-2 md:mr-3 rounded-md p-1 flex items-center justify-center w-8 h-8 md:w-10 md:h-10" 
              style="background-color: {tech.color}"
            >
              <div class="w-5 h-5 md:w-6 md:h-6">
                {@html tech.icon}
              </div>
            </div>
            <div>
              <h3 class="font-medium text-sm md:text-base">{tech.name}</h3>
              <p class="text-gray-400 text-xs">{tech.description}</p>
            </div>
          </div>
        {/each}
      </div>
    </section>
    
    <!-- My Projects section -->
    <section class="mb-8">
      <div in:fade={{ duration: 400 }}>
        <h2 class="text-lg md:text-xl font-bold mb-2">My Projects</h2>
        <p class="text-gray-400 text-xs md:text-sm mb-4">Here are some of the projects I've built and am currently working on. Check them out!</p>
      </div>
      
      <div 
        class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4"
        use:staggerChildren={{ delay: 200 }}
      >
        <!-- Project Card 1 -->
        <div class="bg-zinc-900 rounded-lg overflow-hidden flex flex-col h-full transform transition-all duration-500 hover:scale-105 hover:shadow-lg hover:shadow-blue-500/20">
          <div class="h-48 bg-zinc-800 relative overflow-hidden">
            <!-- Project image with hover effect -->
            <img 
              src="/aur.png" 
              alt="AUR Project Screenshot" 
              class="w-full h-full object-cover transition-transform duration-700 hover:scale-110"
            />
          </div>
          <div class="p-4 flex-grow">
            <h3 class="text-lg font-medium mb-2">Australian Unofficial Cubing Records</h3>
            <p class="text-gray-400 text-sm mb-3">A modern at-home-cubing records tracker for my home country, Australia</p>
            <div class="flex flex-wrap gap-2 mb-4">
              <span class="text-xs bg-blue-900/30 text-blue-400 px-2 py-1 rounded">TypeScript</span>
              <span class="text-xs bg-purple-900/30 text-purple-400 px-2 py-1 rounded">Vercel</span>
              <span class="text-xs bg-amber-900/30 text-amber-400 px-2 py-1 rounded">SvelteKit</span>
              <span class="text-xs bg-green-900/30 text-green-400 px-2 py-1 rounded">Supabase</span>
            </div>
          </div>
          <div class="p-4 pt-0 flex gap-3">
            <a 
              href="https://cubingunofficial.com/" 
              class="text-sm text-blue-400 hover:text-blue-300 flex items-center transition-all duration-300 hover:translate-x-1"
            >
              <span class="mr-1">Demo</span>
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
              </svg>
            </a>
          </div>
        </div>
        <div class="bg-zinc-900 rounded-lg overflow-hidden flex flex-col h-full transform transition-all duration-500 hover:scale-105 hover:shadow-lg hover:shadow-blue-500/20">
          <div class="h-48 bg-zinc-800 relative overflow-hidden">
            <!-- Project image with hover effect -->
            <img 
              src="/cog.webp" 
              alt="DynoScript" 
              class="w-full h-full object-cover transition-transform duration-700 hover:scale-110"
            />
          </div>
          <div class="p-4 flex-grow">
            <h3 class="text-lg font-medium mb-2">DynoScript</h3>
            <p class="text-gray-400 text-sm mb-3">A unique esolang meant to be as configurable as possible, and adapt to your preferences. Just a cool interpreter project im working on.</p>
            <div class="flex flex-wrap gap-2 mb-4">
              <span class="text-xs bg-blue-900/30 text-blue-400 px-2 py-1 rounded">GoLang</span>
            </div>
          </div>
          <div class="p-4 pt-0 flex gap-3">
            <a 
              href="https://cubingunofficial.com/" 
              class="text-sm text-blue-400 hover:text-blue-300 flex items-center transition-all duration-300 hover:translate-x-1"
            >
              <span class="mr-1">Demo</span>
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
              </svg>
            </a>
          </div>
        </div>
        <div class="bg-zinc-900 rounded-lg overflow-hidden flex flex-col h-full transform transition-all duration-500 hover:scale-105 hover:shadow-lg hover:shadow-blue-500/20">
          <div class="h-48 bg-zinc-800 relative overflow-hidden">
            <!-- Project image with hover effect -->
            <img 
              src="/particle-life.png" 
              alt="AUR Project Screenshot" 
              class="w-full h-full object-cover transition-transform duration-700 hover:scale-110"
            />
          </div>
          <div class="p-4 flex-grow">
            <h3 class="text-lg font-medium mb-2">Particle Life</h3>
            <p class="text-gray-400 text-sm mb-3">A simple engine for particle life built in go with Ebitengine. Super interesting small project, was a lot of fun.</p>
            <div class="flex flex-wrap gap-2 mb-4">
              <span class="text-xs bg-blue-900/30 text-blue-400 px-2 py-1 rounded">GoLang</span>
              <span class="text-xs bg-amber-900/30 text-amber-400 px-2 py-1 rounded">Ebitengine</span>
            </div>
          </div>
          <div class="p-4 pt-0 flex gap-3">
            <a 
              href="https://cubingunofficial.com/" 
              class="text-sm text-blue-400 hover:text-blue-300 flex items-center transition-all duration-300 hover:translate-x-1"
            >
              <span class="mr-1">Demo</span>
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
              </svg>
            </a>
          </div>
        </div>
        
        <!-- Project Cards 2 & 3 with similar animations -->
        <!-- Other project cards follow the same pattern -->
      </div>
    </section>
    
    <!-- Contact Me section -->
    <section class="mb-8 bg-zinc-900 rounded-lg p-6 shadow-lg" in:fade={{ duration: 800, delay: 200 }}>
      <h2 class="text-lg md:text-xl font-bold mb-4">Contact Me</h2>
      
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6" in:fade={{ duration: 800, delay: 200 }}>
        <div>
          <p class="text-gray-300 mb-4">Feel free to reach out if you have any questions or want to collaborate on a project! Heres a quick summary on the right if you want to just skip to the point.</p>
          
          <div class="flex flex-col space-y-3">
            <a 
              href="mailto:ethanannane@proton.me" 
              class="flex items-center text-blue-400 hover:text-blue-300 transition-all duration-300 group"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
              </svg>
              <span class="group-hover:translate-x-1 transition-transform duration-300">ethanannane@proton.me</span>
            </a>
            
            <a 
              href="https://github.com/clamared" 
              target="_blank" 
              rel="noopener noreferrer" 
              class="flex items-center text-blue-400 hover:text-blue-300 transition-all duration-300 group"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"></path>
              </svg>
              <span class="group-hover:translate-x-1 transition-transform duration-300">GitHub</span>
            </a>

          </div>
        </div>
        
        <div class="bg-zinc-800 rounded-lg p-4">
          <h3 class="text-md font-semibold mb-3">About Me</h3>
          <p class="text-gray-300 mb-3">Full-stack developer with a love for building awesome stuff and performant applications.</p>
          
          <div class="space-y-2">
            <div class="flex items-start">
              <MapPin class="h-5 w-5 mr-2 text-gray-400 mt-0.5" />
              <span class="text-gray-300">Bunbury, WA, Australia</span>
            </div>
            
            <div class="flex items-center">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 13.255A23.931 23.931 0 0112 15c-3.183 0-6.22-.62-9-1.745M16 6V4a2 2 0 00-2-2h-4a2 2 0 00-2 2v2m4 6h.01M5 20h14a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
              </svg>
              <span class="text-gray-300">Not employed, not seeking</span>
            </div>
            
            <div class="flex items-center">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
              </svg>
              <span class="text-gray-300">2 years of building cool things</span>
            </div>
          </div>
          
          <div class="mt-4 pt-4 border-t border-zinc-700">
            <h4 class="text-sm font-medium mb-2">Connect with me:</h4>
            <div class="flex space-x-3">
              <a href="https://github.com/clamared" target="_blank" rel="noopener noreferrer" class="p-2 bg-zinc-700 rounded-full hover:bg-zinc-600 transition-colors duration-300">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-300" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
                </svg>
              </a>
              <a href="mailto:ethanannane@proton.me" class="p-2 bg-zinc-700 rounded-full hover:bg-zinc-600 transition-colors duration-300">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                </svg>
              </a>
            </div>
          </div>
        </div>
      </div>
    </section>
    
  </div>
</div>

<style>
  /* Enhanced stars background with animation */
  .stars-bg {
    background-color: #000;
    background-image: 
      radial-gradient(white, rgba(255,255,255,.2) 2px, transparent 2px),
      radial-gradient(white, rgba(255,255,255,.15) 1px, transparent 1px);
    background-size: 50px 50px, 30px 30px;
    background-position: 0 0, 25px 25px;
    opacity: 0.1;
    transition: transform 0.5s ease-out;
  }
  
  /* Animated placeholder styles for icons */
  .icon-placeholder {
    border: 1px dashed #666;
    border-radius: 4px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 10px;
    color: #999;
    position: relative;
    overflow: hidden;
  }
  
  .icon-placeholder::after {
    content: attr(data-icon);
    position: absolute;
  }
  
  .icon-placeholder::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.1), transparent);
    animation: shine 2s infinite;
  }
  
  @keyframes shine {
    0% { left: -100%; }
    20% { left: 100%; }
    100% { left: 100%; }
  }
  
  /* Enhanced wave animation */
  .wave-emoji {
    display: inline-block;
    animation: wave 2.5s infinite;
    transform-origin: 70% 70%;
    filter: drop-shadow(0 0 1px rgba(59, 130, 246, 0.5));
  }
  
  @keyframes wave {
    0% { transform: rotate( 0.0deg); }
    10% { transform: rotate(14.0deg); }
    20% { transform: rotate(-8.0deg); }
    30% { transform: rotate(14.0deg); }
    40% { transform: rotate(-4.0deg); }
    50% { transform: rotate(10.0deg); }
    60% { transform: rotate( 0.0deg); }
    100% { transform: rotate( 0.0deg); }
  }
  
  /* Add a subtle pulse animation to tech icons */
  @keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.05); }
    100% { transform: scale(1); }
  }
</style>

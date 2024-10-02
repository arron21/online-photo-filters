<template>
  <div class="frame">
    <h1>About</h1>
    <p>
      This project was created to offer a free and simple online photo filter application with no hidden costs or conditions.
    </p>
    <p>Your privacy is fully protected—your photos never leave your device. All filters are applied locally on your computer, ensuring 100% client-side processing.</p>
    <p>While the website does feature ads to cover hosting expenses, there are no additional fees or data tracking involved.</p>
    <h3>Instructions</h3>
    <ul>
      <li>
        Filter Cycling: Use the <kbd>Z</kbd> and <kbd>X</kbd> keys to cycle through different filters applied to the "light leak" effects.
      </li>
      <li>
        Blend Modes: Press the <kbd>left</kbd> and <kbd>right</kbd> arrow keys to navigate through various blend modes, which alter the appearance of the "light leak" effects.
      </li>
      <li>
        Customization: Combine filters and blend modes to create your own unique visual effects.
      </li>
      <li>
        Basic Filters: If you prefer, you can select a pre-made "Basic Filter" and click "Download" to save the edited image to your device.
      </li>
    </ul>
    </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}

kbd {
  background-color: #eee;
  border-radius: 3px;
  border: 1px solid #b4b4b4;
  box-shadow:
    0 1px 1px rgba(0, 0, 0, 0.2),
    0 2px 0 0 rgba(255, 255, 255, 0.7) inset;
  color: #333;
  display: inline-block;
  font-size: 0.85em;
  font-weight: 700;
  line-height: 1;
  padding: 2px 4px;
  white-space: nowrap;
}

</style>

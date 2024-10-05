<template>
  <div class="frame">
    <h1>About</h1>
    <p>
      This project was created to offer a free and simple online photo filter application with no hidden costs or conditions.
    </p>
    <h2>Instructions</h2>
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

<style scoped>
.frame {
  margin: 1rem;
}
h1,h2 {
  margin: 1rem 0;
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

# Online Photo Filter

this project was a fun way to play around with vue and typescript.

I learned some interesting ways to code using vue. Coming from an Angular background I found a lot of the same patterns and it was fun to play with them in vue.

I refactored what I call the "Frame" which is just a div that contains the image and the controls. in the PhotoFrame.vue file. The PhotoFrame component is mostly a "dumb" component that includes the logic for the download button. Passing props to it was as easy as defining them like so

```
    const props = defineProps<{
        userImage: any,
        filterImage: any,
        filterPath: string,
        globalFilterMode: string | undefined,
        globalBlendMode: string,
    }>()
```
I would call the PhotoFrame component like this
```
    <PhotoFrame 
    :filterImage="n" 
    :filterPath="'/lightleak1/Multi-'" 
    :userImage="selectedImg" 
    :globalBlendMode="globalBlendMode" 
    :globalFilterMode="globalFilterMode" />
```

I am sure I could have improved this a bit by combining the filterImage and the filterPath, but I like it as it is for now

Passing in globalBlendMode and globalFilterMode worked like a charm.
Those variables are defined on the PhotoView component (which is basically the main component) and the syntax is simple

`const globalBlendMode = ref('soft-light')`

Something that was a little confusing was updating certain variables values. For example this function was strange because selectedImg was no enough, I had to target the /value property of the ref object.

```
const onSetSelectedImg = (img: string) => {
  const file = event.target.files[0];
  if (file) {
    selectedImg.value = URL.createObjectURL(file);
  }
}
```
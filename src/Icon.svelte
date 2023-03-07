<script>
    export let name;
    export let width = "1rem";
    export let height = "1rem";
    export let focusable = false;
    let icons = [
        {
          box: 24,
          name: "expand-more",
          svg: `<path d="M12 15.375L6 9.37505L7.075 8.30005L12 13.25L16.925 8.32505L18 9.40005L12 15.375Z" fill="white"/>`

        },
        {
          box: 24,
          name: "expand-less",
          svg: `<path d="M7.075 15.375L6 14.3L12 8.30005L18 14.275L16.925 15.35L12 10.425L7.075 15.375Z" fill="white"/>`
        }
    ];
    let displayIcon = icons.find((e) => e.name === name);
</script>
<svg
  class={$$props.class}
  {focusable}
  {width}
  {height}
  viewBox="0 0 {displayIcon.box} {displayIcon.box}">{@html displayIcon.svg}</svg>
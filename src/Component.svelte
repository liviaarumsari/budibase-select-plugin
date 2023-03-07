<script>
  import { getContext } from "svelte"
  import Icon from "./Icon.svelte";

  export let text
  export let selectType

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  const options = [
    { value: "pizza", label: "Pizza", group: "Italian" },
    { value: "pasta", label: "Pasta", group: "Italian" },
    { value: "lasagna", label: "Lasagna", group: "Italian" },
    { value: "sushi", label: "Sushi", group: "Japanese" },
    { value: "ramen", label: "Ramen", group: "Japanese" },
    { value: "tempura", label: "Tempura", group: "Japanese" },
    { value: "taco", label: "Taco", group: "Mexican" },
    { value: "burrito", label: "Burrito", group: "Mexican" },
    { value: "enchilada", label: "Enchilada", group: "Mexican" },
    { value: "hamburger", label: "Hamburger", group: "American" },
    { value: "hot dog", label: "Hot Dog", group: "American" },
    { value: "steak", label: "Steak", group: "American" },
    { value: "schnitzel", label: "Schnitzel", group: "German" },
    { value: "bratwurst", label: "Bratwurst", group: "German" },
    { value: "spätzle", label: "Spätzle", group: "German" },
    { value: "pad thai", label: "Pad Thai", group: "Thai" },
    { value: "curry", label: "Curry", group: "Thai" },
    { value: "tom yum soup", label: "Tom Yum Soup", group: "Thai" },
    { value: "hummus", label: "Hummus", group: "Middle Eastern" },
    { value: "falafel", label: "Falafel", group: "Middle Eastern" },
  ];
  let filteredOptions = options
  let isDropDownOpen = false

  const dropdownClickHandler = () => {
    isDropDownOpen = !isDropDownOpen
  }
  
  function filterOptions(option) {
    return option.label.toLowerCase().includes(searchText.toLowerCase());
  }
  
  $: filteredOptions = options.filter(filterOptions);
  let searchText = "";
  let selectedOptions = [];

  const handleSearchInput = () => {
    return filteredOptions = options.filter(
      (option) =>
        option.label.toLowerCase().includes(searchText.toLowerCase()) ||
        option.group.toLowerCase().includes(searchText.toLowerCase())
    );
  };

  function selectOption(option) {
    if (selectType === "single") {
      selectedOptions = [option];
    } else {
      if (selectedOptions.includes(option)) {
        selectedOptions = selectedOptions.filter(o => o !== option);
      } else {
        selectedOptions = [...selectedOptions, option];
      }
    }
  }

  function removeSelectedOption(option) {
    selectedOptions = selectedOptions.filter((selectedOption) => selectedOption.value !== option.value);
  }

  function groupOptions() {
    const groupedOptions = {};
    options.forEach((option) => {
      if (!groupedOptions[option.group]) {
        groupedOptions[option.group] = [];
      }
      groupedOptions[option.group].push(option);
    });
    return groupedOptions;
  }

  function clearSearch() {
    searchText = "";
  }

</script>

<div use:styleable={$component.styles}>
  <h4>{text}</h4>
  {#if selectedOptions.length > 0}
  <div class="selected-options">
    {#each selectedOptions as selectedOption}
      <span class="selected-option" on:click={() => removeSelectedOption(selectedOption)}>
        {selectedOption.label} &times;
      </span>
    {/each}
  </div>
  {/if}
  <div class="select">
    <div class="search-bar">  
      <input
      class="search-field"
      type="text"
      placeholder="Search..."
      bind:value={searchText}
      on:input={handleSearchInput}
    />
    <button on:click={dropdownClickHandler} class="dropdown">
      {#if isDropDownOpen}
        <Icon name="expand-less" />
      {:else}
      <Icon name="expand-more" />
      {/if}
    </button>
    </div>
    <div class="option-list" style:visibility={isDropDownOpen ? 'visible' : 'hidden'}>
      {#each Object.entries(groupOptions()) as [groupName, groupOptions]}
        <div class="group">
          <h4>{groupName}</h4>
          {#each filteredOptions as option}
            {#if option.group === groupName}
              <div
                class="option"
                on:click={() => selectOption(option)}
                class:selected={selectedOptions.includes(option)}
              >
                {option.label}
              </div>
            {/if}
          {/each}
        </div>
      {/each}
    </div>
    </div>
</div>

<style>
  .select {
    display: flex;
    flex-direction: column;
    position: relative;
    width: 280px;
    margin: 2px;
  }

  .search-bar {
    flex-direction: row;
    position: relative;
    align-items: center;
    justify-content: center;
  }

  .search-field {
    width: auto;
    border-radius: 8px;
    padding: 4px 8px;
    border: #e5e5e5;
  }

  .dropdown {
    background-color: #007aff;
    height: 30px;
    width: 30px;
    border-radius: 16px;
    border: none;
  }
  .option-list {
    background-color: white;
    box-shadow: 4px 4px 17px 2px rgba(0,0,0,0.23);
    width: fit-content;
    padding: 12px 24px;
  }

  .selected-options {
    display: flex;
    flex-wrap: wrap;
    margin-bottom: 8px;
  }

  .selected-option {
    display: flex;
    align-items: center;
    background-color: #007aff;
    color: white;
    padding: 4px 8px;
    margin-right: 8px;
    margin-bottom: 8px;
    border-radius: 16px;
    cursor: pointer;
  }

  .option {
    padding: 8px;
    cursor: pointer;
  }

  .option.selected {
    background-color: #007aff;
    color: white;
  }

  .group {
    margin-bottom: 16px;
  }

  h4 {
    margin: 0;
    margin-bottom: 8px;
    font-size: 16px;
  }
</style>

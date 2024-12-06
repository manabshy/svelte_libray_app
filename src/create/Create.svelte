<script>
  import Header from "../library/common/Header.svelte";
  import BackButtonRow from "../library/common/BackButtonRow.svelte";
  import BookCover from "../library/common/BookCover.svelte";
  import Button from "../library/common/Button.svelte";
  import { httpPost } from "../library/common/api";
  import TextInput from "./TextInput.svelte";
  import { navigate } from "svelte-routing";
  let title = "";
  let author = "";
  let cover = "";
  let about = "";

  $: book = { title, author, cover, about }
 
  // Destiny operator
  $: console.log({ title, author, cover, about });

  async function handleSubmit(event) {
    console.log('submitted')
    function getRandomInt(min,max) {
        min = Math.ceil(min);
        max = Math.floor(max);
        return Math.floor(Math.random() * (max - min + 1)) + min;
    }
     const newBook = {
        ...book,
        variation: getRandomInt(0, 2),
        favorite:false
    }
    const { ok } = await httpPost( '/', newBook)
    if (ok) {
        navigate('/')
    }
  }
</script>

<BackButtonRow />

<Header element="h1" size="large">Create</Header>

<form on:submit|preventDefault={handleSubmit}>
  <div class="fields">
    <TextInput label="Title" bind:value={title} />
    <TextInput label="Author" bind:value={author} />
    <TextInput label="Cover" bind:value={cover} />
    <TextInput label="About" bind:value={about} multiline />
    <div>
        <Button>Save</Button>
    </div>
  </div>

  <div>
    <Header>Preview</Header>
    <div class="preview">
        <BookCover book} />
    </div>
  </div>
</form>

<style>
  form {
    display: grid;
    grid-auto-rows: auto;
    grid-template-columns: 1fr;
    gap: var(--spacingXLarge);
  }
  .fields {
    display: grid;
    grid-auto-rows: auto;
    gap: var(--spacingMedium);
  }
  .preview {
    display: grid;
    grid-template-columns: minmax(20vw, 10rem);
    grid-template-rows: minmax(32vw, 16rem);
  }
  @media (min-width: 48rem) {
    form {
      grid-template-columns: 60vw 20vw;
    }
  }
</style>

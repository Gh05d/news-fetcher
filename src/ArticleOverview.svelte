<script>
  export let title;
  export let link;
  export let picture;
  export let description;
</script>

<style lang="scss">
  section {
    display: flex;
    flex-flow: column;
    justify-content: space-between;
    box-shadow: 2px 2px rgba(0, 0, 0, 0.14);
    height: 350px;

    img {
      width: 250px;
      height: 100px;
      object-fit: cover;
    }

    p {
      text-overflow: ellipsis;
      // white-space: nowrap;
      overflow: hidden;
    }
  }

  a {
    outline: none;
    transition: all 300ms ease-in-out;

    &:hover,
    &:focus {
      transform: translate(5px, -5px);
      background-color: #f5f5f5;
    }
  }
</style>

<a href={link}>
  <section>
    <img src={picture} alt="Ich bin nicht blind" />
    <h1>{title}</h1>
    <p>{description}</p>
  </section>
</a>

<script lang="ts">
  // List your forms here with display name and file URL
  const forms = [
    { name: "Form 1", url: "/forms/membership.pdf" },
    { name: "Form 2", url: "/forms/event-registration.pdf" },
    { name: "Form 3", url: "/forms/liability-waiver.pdf" },
    { name: "Form 4", url: "/forms/volunteer-signup.pdf" },
    { name: "Form 5", url: "/forms/feedback.pdf" },
  ];
</script>

<style>
  main {
    max-width: 1000px;
    margin: 3rem auto 6rem;
    padding: 0 1rem;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: #2a423d;
  }
  h1 {
    font-size: 3.5rem;
    font-weight: 900;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: #264d40;
    text-align: center;
    margin-bottom: 2rem;
    text-shadow:
      0 0 8px #a3c4f3bb,
      0 0 16px #a3c4f355;
  }
  ul {
    list-style: none;
    padding: 0;
    margin: 0 auto;
    max-width: 700px;
  }
  li {
    background: linear-gradient(135deg, #cde3f7, #a0b8d9);
    border: 4px solid #5a6f54;
    border-radius: 25px;
    padding: 1rem 2rem;
    margin-bottom: 1.5rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: 0 0 30px 6px rgba(90, 111, 84, 0.3);
  }
  .form-name {
    margin-right: 2rem;
    font-size: 1.4rem;
    font-weight: 700;
    user-select: text;
  }
  .buttons {
    display: flex;
    gap: 1rem;
  }
  button, a.button-link {
    background: #264d40;
    color: #d9f0ff;
    border: none;
    border-radius: 18px;
    padding: 0.6rem 1.4rem;
    font-weight: 700;
    cursor: pointer;
    box-shadow: 0 0 12px 3px #a3c4f3aa;
    text-decoration: none;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    transition: background-color 0.3s ease;
  }
  button:hover, a.button-link:hover {
    background: #3b6e47;
  }
</style>

<main>
  <h1>Forms</h1>

  <ul>
    {#each forms as form}
      <li>
        <span class="form-name">{form.name}</span>
        <div class="buttons">
          <a
            class="button-link"
            href={form.url}
            target="_blank"
            rel="noopener noreferrer"
            aria-label={`Open ${form.name} in a new tab`}
          >
            Open
          </a>
          <a
            class="button-link"
            href={form.url}
            download
            aria-label={`Download ${form.name}`}
          >
            Download
          </a>
        </div>
      </li>
    {/each}
  </ul>
</main>

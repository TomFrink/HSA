<script>
  let overFiftyfive = false;
  let family = false;
  let paychecks = 26;
  let employerContribution = 0;
  let employeeContribution = 0;
  $: totalContribution = employerContribution + employeeContribution;
</script>

<style>
  /* layout */
  .parent {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: 0.5fr 0.5fr 1fr 0.25fr;
    grid-template-areas:
      "head head"
      "info info"
      "input output"
      "foot foot";
    grid-column-gap: 20px;
    grid-row-gap: 10px;
    text-align: center;
  }
  @media only screen and (max-width: 700px) {
    .parent {
      grid-template-columns: 1fr;
      grid-template-rows: 0.5fr 0.5fr 1fr 1fr 0.25fr;
      grid-template-areas:
        "head"
        "info"
        "input"
        "output"
        "foot";
    }
  }
  .head {
    grid-area: head;
  }
  .info {
    grid-area: info;
    background-color: var(--light-cyan);
  }
  .input {
    grid-area: input;
    background-color: var(--burnt-sienna);
    color: var(--blue);
    font-size: 24px;
  }
  .output {
    grid-area: output;
    background-color: var(--blue);
    color: var(--burnt-sienna);
    font-size: 24px;
  }
  .foot {
    grid-area: foot;
    color: var(--light-cyan);
  }
  /* font */
  @font-face {
    font-family: "Anton", sans-serif;
    src: local("Anton"), local("Anton"),
      url(https://fonts.googleapis.com/css2?family=Anton&display=swap);
  }
  h1 {
    font-family: "Anton", sans-serif;
    font-size: 64px;
    color: var(--pale-blue);
  }
  .foot a:any-link {
    color: var(--burnt-sienna);
  }
  /* colors */
  * {
    --blue: #001f54;
    --pale-blue: #98c1d9ff;
    --light-cyan: #e0fbfcff;
    --burnt-sienna: #ee6c4dff;
    --gunmetal: #293241ff;
  }
  main {
    background: var(--gunmetal);
  }
</style>

<main class="parent">
  <header class="head">
    <h1>2021 HSA Contribution Calculator</h1>
  </header>

  <!-- info -->
  <article class="info">
    <p>
      There are Annual Contribution Limits for Health Savings Accounts (HSA),
      According to
      <a href="https://www.irs.gov/pub/irs-drop/rp-20-32.pdf">
        this IRS.gov website</a>.
      <br />
      These limits are adjusted yearly for inflation.
    </p>
    <p>
      These limits for 2021 are:<br />
      <strong>$3,600 for self-only HDHP coverage<br />
        $7,200 for family HDHP coverage</strong>
    </p>
    <p>
      If you are age 55 or older at the end of this tax year, your contribution
      limit is increased by $1,000.<br />
    </p>
  </article>
  <hr />

  <!-- input -->
  <article class="input">
    <label>
      <input type="checkbox" bind:checked={overFiftyfive} />
      55 or older this year?
    </label>

    <label>
      <input type="checkbox" bind:checked={family} />
      Family Insurance?
    </label>

    <!-- <p>How frequently do you get paid?</p>
    <label>
      <input type="radio" bind:group={paychecks} value={52} />
      Weekly
    </label>

    <label>
      <input type="radio" bind:group={paychecks} value={26} />
      Bi-weekly
    </label>

    <label>
      <input type="radio" bind:group={paychecks} value={12} />
      Monthly
    </label> -->

    <p>How much is your employer going to contribute this year?</p>
    <label>
      <input
        type="number"
        bind:value={employerContribution}
        min="0"
        max="2000" />
      <input
        type="range"
        bind:value={employerContribution}
        min="0"
        max="2000" />
    </label>

    <p>How much are you going to contribute this year?</p>

    {#if family && overFiftyfive}
      <label>
        <input
          type="number"
          bind:value={employeeContribution}
          min="0"
          max="8200" />
        <input
          type="range"
          bind:value={employeeContribution}
          min="0"
          max="8200" />
      </label>
    {:else if family}
      <label>
        <input
          type="number"
          bind:value={employeeContribution}
          min="0"
          max="7200" />
        <input
          type="range"
          bind:value={employeeContribution}
          min="0"
          max="7200" />
      </label>
    {:else if overFiftyfive}
      <label>
        <input
          type="number"
          bind:value={employeeContribution}
          min="0"
          max="4600" />
        <input
          type="range"
          bind:value={employeeContribution}
          min="0"
          max="4600" />
      </label>
    {:else}
      <label>
        <input
          type="number"
          bind:value={employeeContribution}
          min="0"
          max="3600" />
        <input
          type="range"
          bind:value={employeeContribution}
          min="0"
          max="3600" />
      </label>
    {/if}
  </article>

  <!-- output -->
  <article class="output">
    <p>
      {#if overFiftyfive}
        You're 55 or older this year.
      {:else}You are under 55.{/if}
    </p>
    <p>
      {#if family}
        You have a Family Insurace Plan.
      {:else}You have a Single Insurance Plan.{/if}
    </p>

    <h2>Total Annual Contribution of $ {totalContribution}</h2>

    <p>
      Weekly Contribution: $
      {(employeeContribution / 52).toFixed(2)}
      <br />
      Bi-Weekly Contribution: $
      {(employeeContribution / 26).toFixed(2)}
      <br />
      Monthly Contribution: $
      {(employeeContribution / 12).toFixed(2)}
    </p>
  </article>

  <!-- closing -->
  <footer class="foot">
    <p>Made By <a href="https://github.com/TomFrink">Tom Frink</a></p>
  </footer>
</main>

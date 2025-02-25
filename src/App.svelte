<script>
  let nome = $state("");
  let cognome = $state("");
  let presentazione = $state("");
  let age = $state("");
  let paese = $state("");
  let lavoro = $state("");
  let isRemote = $state(false);
  let esperienza = $state("");
  let responseMessage = $state("");

  const paesi = [
    { value: "it", name: "Italia" },
    { value: "sp", name: "Spagna" },
    { value: "fr", name: "Francia" },
    { value: "de", name: "Germania" },
    { value: "al", name: "Altro" },
  ];

  const lavori = [
    { value: "am", name: "Amazon" },
    { value: "gl", name: "Google" },
    { value: "mc", name: "Microsoft" },
    { value: "al", name: "Altro" },
  ];

  async function inviaForm(event) {
    event.preventDefault();

    try {
      const res = await fetch("https://server-backend-express-production.up.railway.app/utente", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          nome,
          cognome,
          age,
          presentazione,
          paese,
          lavoro,
          isRemote,
          esperienza,
        }),
      });

      if (res.ok) {
        const data = await res.json();
        console.log(data);
        responseMessage = `${data.message || "Risposta ricevuta senza messaggio"}`;
        console.log(responseMessage);
      } else {
        responseMessage = "Errore durante l'invio del form";
        console.log(responseMessage);
      }
    } catch (e) {
      console.log(e);
    }
  }
</script>

<h1>Benvenuto {nome}</h1>
<div class="glass-form">
  <form onsubmit={inviaForm}>
    <input type="text" bind:value={nome} placeholder="Inserisci il tuo nome" />
    <input
      type="text"
      bind:value={cognome}
      placeholder="inserisci il tuo cognome"
    />
    <input type="number" bind:value={age} placeholder="inserisci la tua età" />
    <textarea
      id="presentazione"
      bind:value={presentazione}
      placeholder="scrivi una breve presentazione..."
    ></textarea>
    <label for="provenienza">Seleziona il tuo paese di provenienza:</label>
    <select id="paese-provenienza" bind:value={paese}>
      {#each paesi as paese}
        <option value={paese.value}>{paese.name}</option>
      {/each}
    </select>
    <label for="lavoro">Seleziona il luogo in cui hai lavorato:</label>
    <select id="lavoro-precedente" bind:value={lavoro}>
      {#each lavori as lavoro}
        <option value={lavoro.value}>{lavoro.name}</option>
      {/each}
    </select>
    <label for="remoto">Sei disponibile a lavorare da remoto?</label>
    <input type="checkbox" bind:checked={isRemote} />sì
    <label for="esperienza">Quanta esperienza lavorativa hai?</label>
    <input type="radio" name="exp" value="nessuna" bind:group={esperienza} />
    Nessuna
    <input type="radio" name="exp" value="1-2" bind:group={esperienza} /> 1-2
    anni
    <input type="radio" name="exp" value="3-5" bind:group={esperienza} /> 3-5
    anni
    <input type="radio" name="exp" value="5+" bind:group={esperienza} /> 5+ anni

    <button type="submit">Invia</button>

    {#if responseMessage && responseMessage.length > 0}
      <p>{responseMessage}</p>
    {/if}
  </form>
</div>

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Movie Club</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Work+Sans:wght@400;500;600;700&display=swap');

:root{
  --bg: #14171C;
  --panel: #1E232C;
  --panel-raised: #262C37;
  --gold: #D4A72C;
  --gold-dim: #A9862A;
  --cream: #ECE6D6;
  --cream-dim: #B9B3A3;
  --velvet: #8B3A3A;
  --velvet-bright: #B14848;
  --line: #363D49;
}

*{ box-sizing: border-box; }

body{
  margin:0;
  background: var(--bg);
  color: var(--cream);
  font-family: 'Work Sans', sans-serif;
  min-height: 100vh;
  padding-bottom: 60px;
}

.wrap{
  max-width: 560px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Marquee header */
.marquee{
  padding: 36px 0 22px;
  text-align: center;
  position: relative;
}
.marquee::before, .marquee::after{
  content: "";
  display: block;
  height: 6px;
  margin: 0 auto 18px;
  max-width: 400px;
  background-image: radial-gradient(circle, var(--gold) 1.6px, transparent 1.8px);
  background-size: 14px 6px;
  background-repeat: repeat-x;
  opacity: 0.85;
}
.marquee::after{ margin: 18px auto 0; }
.marquee h1{
  font-family: 'Bebas Neue', sans-serif;
  font-size: 52px;
  letter-spacing: 3px;
  margin: 0;
  color: var(--gold);
  line-height: 1;
}
.marquee p{
  margin: 8px 0 0;
  color: var(--cream-dim);
  font-size: 14px;
}

/* Sections */
.card{
  background: var(--panel);
  border: 1px solid var(--line);
  border-radius: 4px;
  padding: 22px;
  margin-bottom: 20px;
}
.card h2{
  font-family: 'Bebas Neue', sans-serif;
  font-size: 24px;
  letter-spacing: 1.5px;
  margin: 0 0 16px;
  color: var(--cream);
}

/* Pick panel */
.pick-panel{
  text-align: center;
  background: var(--panel-raised);
  border: 1px solid var(--gold-dim);
}
.pick-display{
  min-height: 64px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Bebas Neue', sans-serif;
  font-size: 30px;
  letter-spacing: 1px;
  color: var(--gold);
  padding: 6px 8px;
  word-break: break-word;
}
.pick-display.empty{
  font-family: 'Work Sans', sans-serif;
  font-size: 14px;
  color: var(--cream-dim);
  letter-spacing: 0;
}
.pick-meta{
  font-size: 13px;
  color: var(--cream-dim);
  margin-top: 4px;
  min-height: 18px;
}
button.pick-btn{
  margin-top: 18px;
  background: var(--velvet);
  color: var(--cream);
  border: none;
  border-radius: 3px;
  padding: 14px 28px;
  font-family: 'Work Sans', sans-serif;
  font-weight: 600;
  font-size: 15px;
  letter-spacing: 0.3px;
  cursor: pointer;
  transition: background 0.15s ease;
  width: 100%;
}
button.pick-btn:hover:not(:disabled){ background: var(--velvet-bright); }
button.pick-btn:disabled{ opacity: 0.5; cursor: default; }

/* Hide any accidental anchor/permalink affordances. */
    h1 a, h2 a, h3 a, p a, .card a, .marquee a, .history-row a {
      color: inherit;
      text-decoration: none;
    }
    h1 a::after, h2 a::after, h3 a::after, p a::after, .card a::after, .marquee a::after, .history-row a::after {
      content: none !important;
      display: none !important;
    }

    .draw-timer{
  margin-top: 12px;
  font-size: 13px;
  color: var(--gold);
  font-weight: 600;
  letter-spacing: 0.2px;
}

.pick-auth{
  display: flex;
  gap: 10px;
  margin-top: 12px;
}
.pick-auth input{
  flex: 1;
  min-width: 0;
  background: var(--bg);
  border: 1px solid var(--line);
  border-radius: 3px;
  padding: 11px 12px;
  color: var(--cream);
  font-family: 'Work Sans', sans-serif;
  font-size: 14px;
}
.pick-auth input::placeholder{ color: #666f7d; }
.pick-auth input:focus{ outline: none; border-color: var(--gold-dim); }
.pick-status{
  min-height: 18px;
  margin-top: 8px;
  font-size: 12.5px;
  color: var(--cream-dim);
}
.pick-status.error{ color: var(--velvet-bright); }

/* Add form */
.add-form{
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.add-form .row{ display: flex; gap: 10px; }
.add-form input{
  flex: 1;
  background: var(--bg);
  border: 1px solid var(--line);
  border-radius: 3px;
  padding: 11px 12px;
  color: var(--cream);
  font-family: 'Work Sans', sans-serif;
  font-size: 14px;
}
.add-form input::placeholder{ color: #666f7d; }
.add-form input:focus{ outline: none; border-color: var(--gold-dim); }
.add-form button{
  background: var(--gold);
  color: #1a1a1a;
  border: none;
  border-radius: 3px;
  padding: 12px 16px;
  font-family: 'Work Sans', sans-serif;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  transition: background 0.15s ease;
}
.add-form button:hover{ background: #e3ba3e; }
.form-error{
  font-size: 12.5px;
  color: var(--velvet-bright);
  margin-top: 8px;
  min-height: 16px;
}
.storage-warning{
  font-size: 12.5px;
  color: var(--gold-dim);
  text-align: center;
  margin: -6px 0 18px;
  padding: 8px 12px;
  border: 1px solid var(--line);
  border-radius: 3px;
  background: var(--panel);
}

/* List */
.list-header{
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  margin-bottom: 16px;
}
.count-badge{
  font-size: 13px;
  color: var(--cream-dim);
}
.movie-row{
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  padding: 13px 0;
  border-top: 1px dashed var(--line);
}
.movie-row:first-of-type{ border-top: none; }
.movie-info{ min-width: 0; }
.movie-title{
  font-size: 15px;
  font-weight: 600;
  color: var(--cream);
  overflow-wrap: break-word;
}
.movie-sub{
  font-size: 12.5px;
  color: var(--cream-dim);
  margin-top: 2px;
}
.remove-btn{
  background: none;
  border: 1px solid var(--line);
  color: var(--cream-dim);
  border-radius: 3px;
  width: 30px;
  height: 30px;
  flex-shrink: 0;
  cursor: pointer;
  font-size: 16px;
  line-height: 1;
  transition: border-color 0.15s ease, color 0.15s ease;
}
.remove-btn:hover{ border-color: var(--velvet-bright); color: var(--velvet-bright); }

.empty-state{
  color: var(--cream-dim);
  font-size: 14px;
  text-align: center;
  padding: 10px 0;
}

/* History */
.history-row{
  display: flex;
  justify-content: space-between;
  gap: 12px;
  padding: 10px 0;
  border-top: 1px dashed var(--line);
  font-size: 13.5px;
}
.history-row:first-of-type{ border-top: none; }
.history-title{ color: var(--cream); font-weight: 500; }
.history-date{ color: var(--cream-dim); white-space: nowrap; }

.status-line{
  text-align: center;
  font-size: 12.5px;
  color: var(--cream-dim);
  margin-top: -6px;
  margin-bottom: 18px;
  min-height: 16px;
}

.footer-note{
  text-align: center;
  color: #565e6b;
  font-size: 12px;
  margin-top: 8px;
}
</style>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

</head>
<body>

<div class="wrap">

  <div class="marquee">
    <h1>MOVIE CLUB</h1>
    <p>One list. Everyone adds. One draw every two weeks.</p>
  </div>

  <div class="storage-warning" id="storageWarning" style="display:none;">
    This preview isn't saving between page loads — everything still works, but publish the app to get a shareable link that syncs for everyone.
  </div>

  <div class="card pick-panel">
    <h2>Current Pick</h2>
    <div class="pick-display empty" id="pickDisplay">No movie selected yet</div>
    <div class="pick-meta" id="pickMeta"></div>
    <div class="draw-timer" id="drawTimer">Ready for the first draw.</div>
    <div class="pick-auth">
      <input type="password" id="pickPasscode" placeholder="Passcode" autocomplete="off">
    </div>
    <button class="pick-btn" id="pickBtn" disabled>Pick a movie</button>
    <div class="pick-status" id="pickStatus">Passcode required to pick.</div>
  </div>

  <div class="card">
    <h2>Add a Movie</h2>
    <div class="add-form" id="addForm">
      <input type="text" id="titleInput" placeholder="Movie title">
      <div class="row">
        <input type="text" id="nameInput" placeholder="Your name">
        <button type="button" id="addBtn">Add</button>
      </div>
    </div>
    <div class="form-error" id="formError"></div>
  </div>

  <div class="card">
    <div class="list-header">
      <h2 style="margin:0;">The List</h2>
      <span class="count-badge" id="countBadge">0 movies</span>
    </div>
    <div class="pick-auth">
      <input type="password" id="removalPasscode" placeholder="Removal password" autocomplete="off">
    </div>
    <div class="pick-status" id="removalStatus">Password required to remove a movie.</div>
    <div id="movieList">
      <div class="empty-state">No movies yet — be the first to add one.</div>
    </div>
  </div>

  <div class="card" id="historyCard" style="display:none;">
    <h2>Past Picks</h2>
    <div id="historyList"></div>
  </div>

  <div class="footer-note">Everyone who opens this page shares the same list.</div>

</div>

<script>
/* ============================================================
   SUPABASE CONFIGURATION
   ============================================================

   Get these from:

   Supabase Dashboard
   → Project Settings
   → API

   Use:
   Project URL
   anon / publishable key

   DO NOT use your service_role key here.
   ============================================================ */

const SUPABASE_URL = 'https://ttrvdzqudkmhzsvdpkrp.supabase.co';
const SUPABASE_ANON_KEY = 'sb_publishable_lD_iraI2h3YX32AAz0N4Fg_LeSZNNuk';

const { createClient } = supabase;

const db = createClient(
  SUPABASE_URL,
  SUPABASE_ANON_KEY
);


/* ============================================================
   APP STATE
   ============================================================ */

let data = {
  movies: [],
  history: []
};

let spinning = false;
let countdownTimer = null;


/* ============================================================
   LOAD DATA FROM SUPABASE
   ============================================================ */

async function loadData() {

  try {

    const [moviesResult, historyResult] = await Promise.all([
      db
        .from('movies')
        .select('*')
        .order('added_at', { ascending: true }),

      db
        .from('history')
        .select('*')
        .order('picked_at', { ascending: false })
    ]);


    if (moviesResult.error) {
      throw moviesResult.error;
    }

    if (historyResult.error) {
      throw historyResult.error;
    }


    data.movies = moviesResult.data.map(movie => ({
      id: movie.id,
      title: movie.title,
      addedBy: movie.added_by,
      addedAt: new Date(movie.added_at).getTime()
    }));


    data.history = historyResult.data.map(item => ({
      id: item.id,
      title: item.title,
      addedBy: item.added_by,
      pickedAt: new Date(item.picked_at).getTime(),
      monthLabel: item.month_label
    }));


    return true;

  } catch (error) {

    console.error('Error loading movie club data:', error);

    showError(
      'Unable to connect to the movie club database.'
    );

    return false;
  }
}


function formatCountdown(ms) {
  const totalSeconds = Math.floor(ms / 1000);
  const days = Math.floor(totalSeconds / 86400);
  const hours = Math.floor((totalSeconds % 86400) / 3600);
  const minutes = Math.floor((totalSeconds % 3600) / 60);
  const seconds = totalSeconds % 60;
  return days + 'd ' + String(hours).padStart(2, '0') + 'h ' + String(minutes).padStart(2, '0') + 'm ' + String(seconds).padStart(2, '0') + 's';
}

function startCountdown() {
  if (countdownTimer) clearInterval(countdownTimer);
  countdownTimer = setInterval(() => {
    render();
  }, 1000);
}


/* ============================================================
   RENDER
   ============================================================ */

function render() {




  /* -------------------------
     MOVIE LIST
     ------------------------- */

  const listEl = document.getElementById('movieList');

  const countBadge =
    document.getElementById('countBadge');


  countBadge.textContent =
    data.movies.length +
    (data.movies.length === 1 ? ' movie' : ' movies');


  if (data.movies.length === 0) {

    listEl.innerHTML =
      '<div class="empty-state">' +
      'No movies yet — be the first to add one.' +
      '</div>';

  } else {

    listEl.innerHTML = data.movies.map(movie => `

      <div class="movie-row">

        <div class="movie-info">

          <div class="movie-title">
            ${escapeHtml(movie.title)}
          </div>

          <div class="movie-sub">
            added by ${escapeHtml(movie.addedBy)}
          </div>

        </div>

        <button
          class="remove-btn"
          data-id="${movie.id}"
          title="Remove"
        >
          &times;
        </button>

      </div>

    `).join('');
  }


  /* -------------------------
     CURRENT PICK + DRAW TIMER
     ------------------------- */

  const pickBtn = document.getElementById('pickBtn');
  const pickPasscode = document.getElementById('pickPasscode');
  const pickStatus = document.getElementById('pickStatus');
  const pickDisplay = document.getElementById('pickDisplay');
  const pickMeta = document.getElementById('pickMeta');
  const drawTimer = document.getElementById('drawTimer');

  const latestPick = data.history.length ? data.history[0] : null;
  const nextDrawTime = latestPick ? latestPick.pickedAt + (14 * 24 * 60 * 60 * 1000) : null;
  const cooldownActive = nextDrawTime !== null && Date.now() < nextDrawTime;

  if (latestPick) {
    pickDisplay.classList.remove('empty');
    pickDisplay.textContent = latestPick.title;
    pickMeta.textContent = 'added by ' + latestPick.addedBy;
  } else {
    pickDisplay.classList.add('empty');
    pickDisplay.textContent = 'No movie selected yet';
    pickMeta.textContent = '';
  }

  pickBtn.disabled = data.movies.length < 2 || spinning || cooldownActive;
  pickPasscode.disabled = spinning || cooldownActive;

  if (spinning) {
    pickBtn.textContent = 'Picking…';
    pickStatus.textContent = 'Pick in progress — please don\'t refresh.';
    pickStatus.classList.remove('error');
  } else if (cooldownActive) {
    pickBtn.textContent = 'Next draw coming soon';
    if (!pickStatus.classList.contains('error')) {
      pickStatus.textContent = 'The current movie stays selected until the next draw.';
    }
  } else {
    pickBtn.textContent = latestPick ? 'Reroll / Pick a new movie' : 'Pick a movie';
    if (!pickStatus.classList.contains('error')) {
      pickStatus.textContent = data.movies.length < 2 ? 'Add at least 2 movies to draw.' : 'Enter the passcode to draw.';
    }
  }

  if (nextDrawTime) {
    const remaining = Math.max(0, nextDrawTime - Date.now());
    if (remaining > 0) {
      drawTimer.textContent = 'Next draw in ' + formatCountdown(remaining);
    } else {
      drawTimer.textContent = 'Next draw is ready.';
    }
  } else {
    drawTimer.textContent = 'Ready for the first draw.';
  }

  /* -------------------------
     HISTORY
     ------------------------- */

  const historyCard =
    document.getElementById('historyCard');

  const historyList =
    document.getElementById('historyList');


  // Keep Previous Picks hidden until there is an actual previous pick.
  // The newest history entry is the current pick, so only older entries
  // belong in the Previous Picks section.
  if (data.history.length < 2) {

    historyCard.style.display = 'none';

  } else {

    historyCard.style.display = '';

    historyList.innerHTML =
      data.history.slice(1).map(item => `

        <div class="history-row">

          <span class="history-title">
            ${escapeHtml(item.title)}
          </span>

          <span class="history-date">
            ${escapeHtml(item.monthLabel)}
          </span>

        </div>

      `).join('');
  }
}


/* ============================================================
   HTML ESCAPING
   ============================================================ */

function escapeHtml(str) {

  const div = document.createElement('div');

  div.textContent = str;

  return div.innerHTML;
}


/* ============================================================
   ERROR MESSAGE
   ============================================================ */

function showError(message) {

  const errorEl =
    document.getElementById('formError');

  if (errorEl) {
    errorEl.textContent = message;
  }

  console.error(message);
}


/* ============================================================
   ADD MOVIE
   ============================================================ */

async function addMovie() {

  const titleInput =
    document.getElementById('titleInput');

  const nameInput =
    document.getElementById('nameInput');

  const errorEl =
    document.getElementById('formError');


  const title =
    titleInput.value.trim();

  const name =
    nameInput.value.trim();


  if (!title || !name) {

    errorEl.textContent =
      'Enter both a movie title and your name.';

    return;
  }


  errorEl.textContent = '';


  const addBtn =
    document.getElementById('addBtn');

  addBtn.disabled = true;


  try {

    const { error } = await db
      .from('movies')
      .insert({
        title: title,
        added_by: name
      });


    if (error) {
      throw error;
    }


    titleInput.value = '';

    titleInput.focus();


    await loadData();

    render();


  } catch (error) {

    console.error('Error adding movie:', error);

    errorEl.textContent =
      'Could not add the movie. Please try again.';

  } finally {

    addBtn.disabled = false;
  }
}


/* ============================================================
   REMOVE MOVIE
   ============================================================ */

async function removeMovie(id) {

  const passcodeInput =
    document.getElementById('removalPasscode');

  const removalStatus =
    document.getElementById('removalStatus');

  const passcode =
    passcodeInput.value;

  if (!passcode) {
    removalStatus.textContent =
      'Enter the removal password first.';
    removalStatus.classList.add('error');
    passcodeInput.focus();
    return;
  }

  removalStatus.textContent = 'Removing…';
  removalStatus.classList.remove('error');

  const { error } = await db.rpc(
    'remove_movie',
    {
      p_movie_id: id,
      p_passcode: passcode
    }
  );

  if (error) {
    console.error('Error removing movie:', error);
    passcodeInput.value = '';

    if (String(error.message || '').includes('INVALID_REMOVAL_PASSCODE')) {
      removalStatus.textContent = 'Incorrect removal password.';
    } else {
      removalStatus.textContent =
        'Could not remove the movie. Please try again.';
    }

    removalStatus.classList.add('error');
    return;
  }

  passcodeInput.value = '';
  removalStatus.textContent = 'Movie removed.';
  removalStatus.classList.remove('error');

  await loadData();
  render();
}


/* ============================================================
   MOVIE LIST BUTTON
   ============================================================ */

document
  .getElementById('movieList')
  .addEventListener('click', async (event) => {

    const button =
      event.target.closest('.remove-btn');

    if (!button) return;

    const id =
      button.getAttribute('data-id');

    await removeMovie(id);
  });

/* ============================================================
   ADD BUTTON
   ============================================================ */

document
  .getElementById('addBtn')
  .addEventListener('click', addMovie);


document
  .getElementById('titleInput')
  .addEventListener('keydown', event => {

    if (event.key === 'Enter') {
      addMovie();
    }

  });


document
  .getElementById('nameInput')
  .addEventListener('keydown', event => {

    if (event.key === 'Enter') {
      addMovie();
    }

  });


/* ============================================================
   PICK MOVIE
   ============================================================ */

async function pickMovie() {

  if (spinning || data.movies.length < 2) {
    return;
  }

  const latestPick = data.history.length ? data.history[0] : null;
  if (latestPick && Date.now() < latestPick.pickedAt + (14 * 24 * 60 * 60 * 1000)) {
    document.getElementById('pickStatus').textContent = 'The next draw is not ready yet.';
    return;
  }

  const passcodeInput =
    document.getElementById('pickPasscode');

  const pickStatus =
    document.getElementById('pickStatus');

  const passcode = passcodeInput.value;

  if (!passcode) {
    pickStatus.textContent = 'Enter the passcode first.';
    pickStatus.classList.add('error');
    passcodeInput.focus();
    return;
  }

  spinning = true;
  pickStatus.classList.remove('error');
  render();

  /*
     The actual winner is chosen inside Supabase.
     The passcode is checked server-side, so it is not stored
     in the public GitHub HTML.
  */
  const { data: result, error } = await db.rpc(
    'pick_movie',
    { p_passcode: passcode }
  );

  if (error) {
    console.error('Error picking movie:', error);

    spinning = false;
    passcodeInput.value = '';

    const message = String(error.message || '');

    if (message.includes('INVALID_PASSCODE')) {
      pickStatus.textContent = 'Incorrect passcode.';
    } else if (message.includes('NO_MOVIES')) {
      pickStatus.textContent = 'There are no movies to pick from.';
      await loadData();
    } else if (message.includes('DRAW_NOT_READY')) {
      pickStatus.textContent = 'The next draw is not ready yet.';
      await loadData();
    } else {
      pickStatus.textContent = 'Could not pick a movie. Please try again.';
    }

    pickStatus.classList.add('error');
    render();
    return;
  }

  const winner = Array.isArray(result) ? result[0] : result;

  const display =
    document.getElementById('pickDisplay');

  const meta =
    document.getElementById('pickMeta');

  display.classList.remove('empty');
  meta.textContent = '';

  // Cosmetic spin animation only. The winner has already been
  // safely finalized in Supabase before this animation begins.
  const pool = data.movies.length ? [...data.movies] : [winner];
  const spinDuration = 1800;
  const stepTime = 90;
  const startTime = Date.now();

  await new Promise(resolve => {
    const interval = setInterval(() => {
      const random = pool[Math.floor(Math.random() * pool.length)];
      display.textContent = random.title || random.movie_title;

      if (Date.now() - startTime >= spinDuration) {
        clearInterval(interval);
        resolve();
      }
    }, stepTime);
  });

  display.textContent = winner.title;
  meta.textContent = 'added by ' + winner.added_by;

  passcodeInput.value = '';

  await loadData();

  spinning = false;
  pickStatus.classList.remove('error');
  render();
}

document
  .getElementById('pickBtn')
  .addEventListener('click', pickMovie);

document
  .getElementById('pickPasscode')
  .addEventListener('keydown', event => {
    if (event.key === 'Enter') {
      pickMovie();
    }
  });


/* ============================================================
   REAL-TIME SYNC
   ============================================================

   This is the important part.

   If somebody adds/removes a movie on their phone,
   computer, etc., Supabase tells every other
   open copy of the website to reload.
   ============================================================ */

db
  .channel('movie-club-live')

  .on(
    'postgres_changes',
    {
      event: '*',
      schema: 'public',
      table: 'movies'
    },
    async () => {

      if (!spinning) {

        await loadData();

        render();
      }
    }
  )

  .on(
    'postgres_changes',
    {
      event: '*',
      schema: 'public',
      table: 'history'
    },
    async () => {

      if (!spinning) {

        await loadData();

        render();
      }
    }
  )

  .subscribe();


/* ============================================================
   INITIAL LOAD
   ============================================================ */

async function initialize() {

  const success =
    await loadData();


  if (success) {

    render();

  } else {


  }
}


initialize();
startCountdown();
</script>

</body>
</html>

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
    <p>One list. Everyone adds. One winner a month.</p>
  </div>

  <div class="storage-warning" id="storageWarning" style="display:none;">
    This preview isn't saving between page loads — everything still works, but publish the app to get a shareable link that syncs for everyone.
  </div>

  <div class="card pick-panel">
    <h2>Tonight's Pick</h2>
    <div class="pick-display empty" id="pickDisplay">Add a few movies, then spin</div>
    <div class="pick-meta" id="pickMeta"></div>
    <button class="pick-btn" id="pickBtn" disabled>Pick this month's movie</button>
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
     PICK BUTTON
     ------------------------- */

  const pickBtn =
    document.getElementById('pickBtn');

  pickBtn.disabled =
    data.movies.length < 2 || spinning;


  /* -------------------------
     HISTORY
     ------------------------- */

  const historyCard =
    document.getElementById('historyCard');

  const historyList =
    document.getElementById('historyList');


  if (data.history.length === 0) {

    historyCard.style.display = 'none';

  } else {

    historyCard.style.display = '';

    historyList.innerHTML =
      data.history.map(item => `

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

  try {

    const { error } = await db
      .from('movies')
      .delete()
      .eq('id', id);


    if (error) {
      throw error;
    }


    await loadData();

    render();


  } catch (error) {

    console.error('Error removing movie:', error);

    showError(
      'Could not remove the movie. Please try again.'
    );
  }
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

document
  .getElementById('pickBtn')
  .addEventListener('click', async () => {

    if (
      spinning ||
      data.movies.length < 2
    ) {
      return;
    }


    spinning = true;

    render();


    /*
       Reload immediately before spinning
       so we have the newest possible list.
    */

    await loadData();


    const pool = [...data.movies];


    if (pool.length < 2) {

      spinning = false;

      render();

      return;
    }


    const display =
      document.getElementById('pickDisplay');

    const meta =
      document.getElementById('pickMeta');


    display.classList.remove('empty');

    meta.textContent = '';


    /* -------------------------
       SPIN ANIMATION
       ------------------------- */

    const spinDuration = 1800;

    const stepTime = 90;

    const startTime = Date.now();


    await new Promise(resolve => {

      const interval =
        setInterval(() => {

          const random =
            pool[
              Math.floor(
                Math.random() * pool.length
              )
            ];


          display.textContent =
            random.title;


          if (
            Date.now() - startTime >=
            spinDuration
          ) {

            clearInterval(interval);

            resolve();
          }

        }, stepTime);

    });


    /*
       Select the winner locally.
       We then save that exact movie
       to Supabase.
    */

    const winner =
      pool[
        Math.floor(
          Math.random() * pool.length
        )
      ];


    display.textContent =
      winner.title;

    meta.textContent =
      'added by ' + winner.addedBy;


    const now =
      new Date();


    const monthLabel =
      now.toLocaleDateString(
        undefined,
        {
          month: 'long',
          year: 'numeric'
        }
      );


    try {

      /*
         Add winner to history first.
      */

      const historyResult =
        await db
          .from('history')
          .insert({
            title: winner.title,
            added_by: winner.addedBy,
            picked_at: new Date().toISOString(),
            month_label: monthLabel
          });


      if (historyResult.error) {
        throw historyResult.error;
      }


      /*
         Remove winner from the shared movie list.
      */

      const deleteResult =
        await db
          .from('movies')
          .delete()
          .eq('id', winner.id);


      if (deleteResult.error) {
        throw deleteResult.error;
      }


      /*
         Reload shared data.
      */

      await loadData();

      render();


    } catch (error) {

      console.error(
        'Error saving movie pick:',
        error
      );


      showError(
        'Could not save the movie pick. Please try again.'
      );
    }


    spinning = false;

    render();
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
</script>

</body>
</html>

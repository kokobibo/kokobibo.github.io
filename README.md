<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

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

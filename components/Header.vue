<template>
  <client-only>
    <nav class="uk-navbar-transparent uk-navbar-container uk-margin" uk-navbar>
      <div class="nav-overlay uk-navbar-left">
        <a
          class="uk-navbar-toggle"
          uk-navbar-toggle-icon
          uk-toggle="target: #sidenav"
          href="#"
        ></a>
      </div>
      <div
        class="nav-overlay uk-navbar-center uk-margin-small-top"
        style="z-index: 1"
      >
        <img
          data-src="/images/icon.png"
          class="cursor-pointer"
          width="300"
          uk-img
          @click="$router.push({ path: '/' })"
        />
      </div>
      <div class="uk-navbar-right">
        <a
          class="uk-navbar-toggle"
          uk-search-icon
          uk-toggle="target: .nav-overlay; animation: uk-animation-fade"
          href="#"
        ></a>
      </div>
      <div
        class="nav-overlay uk-navbar-left uk-flex-1 uk-background-secondary uk-background-blend-multiply"
        hidden
      >
        <div class="uk-navbar-item uk-width-expand">
          <form class="uk-search uk-search-navbar uk-width-1-1">
            <input
              class="uk-search-input"
              type="search"
              placeholder="Search..."
              autofocus
            />
          </form>
        </div>

        <a
          class="uk-navbar-toggle"
          uk-close
          uk-toggle="target: .nav-overlay; animation: uk-animation-fade"
          href="#"
        ></a>
      </div>
      <div id="sidenav" uk-offcanvas="mode: push" class="uk-offcanvas">
        <div class="uk-offcanvas-bar uk-padding-remove">
          <ul class="uk-nav uk-margin-medium-top">
            <li class="uk-padding">
              <a
                class="uk-text-large"
                href="#"
                @click="$router.push({ path: '/' })"
                ><span class="material-icons uk-margin-small-right"> home </span
                >Home</a
              >
            </li>
            <li class="uk-padding">
              <a
                class="uk-text-large"
                href="#"
                @click="$router.push({ path: '/artists' })"
                ><span class="material-icons uk-margin-small-right">
                  record_voice_over </span
                >Artists</a
              >
            </li>
            <li class="uk-padding">
              <a
                class="uk-text-large"
                href="#"
                @click="$router.push({ path: '/albums' })"
                ><span class="material-icons uk-margin-small-right">
                  library_music </span
                >Albums</a
              >
            </li>
            <li class="uk-padding">
              <a
                class="uk-text-large"
                href="#"
                @click="$router.push({ path: '/tracks' })"
                ><span class="material-icons uk-margin-small-right">
                  album </span
                >Tracks</a
              >
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </client-only>
</template>

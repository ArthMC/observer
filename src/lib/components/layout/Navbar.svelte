<script lang="ts">
  import ThemeToggle from "./../buttons/ThemeToggle.svelte";
  import AccountButton from "./../buttons/AccountButton.svelte";
  import Home from "./../buttons/Home.svelte";
  import Billing from "./../buttons/Billing.svelte";
  import NewServer from "./../buttons/NewServer.svelte";
  import Settings from "./../buttons/Settings.svelte";
  import { browser } from "$app/environment";
  import { t, locale, locales } from "$lib/scripts/i18n";
  import { goto } from "$app/navigation";
  //sends user to /signin if localstorage token is ""
  if (browser) {
    if (localStorage.getItem("token") == "") {
      goto("/signin");
    }
  }
  let enablePay = true;
  let login = false;
  type NavType = "default" | "welcome";
  if (typeof navigator !== "undefined") {
    console.log(navigator.language);

    //set locale to the browser's language
    locale.set(navigator.language);
  }
  if (typeof window !== "undefined") {
    if (localStorage.getItem("token") == null) {
      localStorage.setItem("token", "");
    }
    if (localStorage.getItem("token") !== "") {
      login = true;
    } else {
      login = false;
    }
  }
  console.log(login);
  export let navType: NavType;
</script>

{#if navType === "default"}
  <div class="navbar bg-base-300 px-4">
    <div class="hidden sm:block flex-1">
      <a class="btn btn-ghost normal-case text-xl invisible sm:visible" href="/"
        ><img src="/images/sitelogo.svg" alt="Arth" width="75" height="75" /></a
      >
    </div>
    <div class="flex-1 md:flex-none space-x-2 navbar-end">
      <ul class="invisible md:visible md:space-x-0 menu menu-horizontal p-0">
        <li><a href="/">{$t("navbar.servers")}</a></li>
        <div>
          {#if typeof window !== "undefined"}
            {#if localStorage.getItem("adminToken") == "placeholder"}
              <li><a href="/settings">{$t("navbar.settings")}</a></li>
            {/if}
          {/if}
        </div>
      </ul>

      <Home />
      <Settings />
      <ThemeToggle />

      {#if enablePay === true}
        <Billing />
      {/if}
      <NewServer />

      <AccountButton loginStatus={login} />
    </div>
  </div>
{:else if navType === "welcome"}
  <div class="navbar fixed justify-between px-6">
    <Home class="md:invisible" />
    <a class="btn btn-ghost normal-case text-xl invisible sm:visible" href="/"
      ><img src="/images/sitelogo.svg" alt="Arth" width="75" height="75" /></a
    >

    <ThemeToggle />
  </div>
{/if}

<script lang="ts">
  import CenteredPage from "$lib/comps/basic/CenteredPage.svelte";
  import Lightswitch from "$lib/comps/basic/Lightswitch.svelte";
  import ModelProviders from "$lib/comps/models/ModelProviders.svelte";
  import { spaceStore } from "$lib/spaces/spaceStore";
  import { isDevMode } from "$lib/stores/devMode";
  import { currentLanguage } from "$lib/stores/txtStore";
  import { SUPPORTED_LANGUAGES, LANGUAGE_NAMES } from "@core/localization/getTexts";
  import { txtStore } from "$lib/stores/txtStore";
</script>

<CenteredPage>
  <h2 class="h2 mb-4">{$txtStore.settingsPage.title}</h2>

  <div class="flex flex-col gap-6 w-full max-w-2xl">
    <div class="card p-4 preset-filled-surface-100-900 border-[1px] border-surface-200-800">
      <h3 class="h4 mb-4">{$txtStore.settingsPage.appearance.title}</h3>
      <div class="space-y-4">
        <Lightswitch />
        <label class="label">
          <span>{$txtStore.settingsPage.appearance.language}</span>
          <select bind:value={$currentLanguage} class="select">
            {#each SUPPORTED_LANGUAGES as lang}
              <option value={lang}>{LANGUAGE_NAMES[lang]}</option>
            {/each}
          </select>
        </label>
      </div>
    </div>

    <div class="card p-4 preset-filled-surface-100-900 border-[1px] border-surface-200-800">
      <h3 class="h4 mb-4">{$txtStore.settingsPage.providers.title}</h3>
      <ModelProviders />
    </div>

    <div class="card p-4 preset-filled-surface-100-900 border-[1px] border-surface-200-800">
      <h3 class="h4 mb-4">{$txtStore.settingsPage.spaces.title}</h3>
      <p class="mb-4">{$txtStore.settingsPage.spaces.spaceCount($spaceStore.length)}</p>
      <a href="/spaces" class="btn preset-filled">{$txtStore.settingsPage.spaces.manageButton}</a>
    </div>

    <div class="card p-4 preset-filled-surface-100-900 border-[1px] border-surface-200-800">
      <h3 class="h4 mb-4">{$txtStore.settingsPage.developers.title}</h3>
      <button class="btn preset-filled" on:click={() => $isDevMode = !$isDevMode}>{$txtStore.settingsPage.developers.toggleDevMode}</button>
    </div>
  </div>
</CenteredPage>

<script lang="ts">
  import type { ModelProvider, ModelProviderConfig } from "@core/models";
  import { onMount } from "svelte";
  import ModelSelectCard from "./ModelSelectCard.svelte";
  import AutoModelSelectCard from "./AutoModelSelectCard.svelte";
  import { currentSpaceStore } from "$lib/spaces/spaceStore";
  import { providers } from "@core/providers";

  let {
    selectedModel,
    onModelSelect,
  }: {
    selectedModel: string | null;
    onModelSelect: (model: string) => void;
  } = $props();

  let setupProviders: {
    provider: ModelProvider;
    config: ModelProviderConfig;
  }[] = $state([]);

  type SelectedPair = {
    providerId: string;
    model: string;
  };

  let selectedPair: SelectedPair | null = $state(null);

  onMount(async () => {
    const configs = $currentSpaceStore?.getModelProviderConfigs();
    if (!configs) return;

    // Keep the providers that have configs
    setupProviders = providers
      .filter((provider) => configs.some((config) => config.id === provider.id))
      .map((provider) => ({
        provider,
        config: configs.find((config) => config.id === provider.id)!,
      }));

    console.log("setupProviders", setupProviders);

    if (selectedModel) {
      const [providerId, model] = selectedModel.split("/");
      selectedPair = { providerId, model };
    }
  });

  function getPairString(pair: SelectedPair) {
    return pair.providerId + "/" + pair.model;
  }

  function onSelect(providerId: string, model: string) {
    selectedPair = { providerId, model };

    selectedModel = getPairString(selectedPair);
    onModelSelect(selectedModel);
  }
</script>

<div class="grid grid-cols-1 gap-2">
  {#if setupProviders.length > 0}
    <AutoModelSelectCard
      selected={selectedPair?.providerId === "auto"}
      {onSelect}
    />
  {/if}
  {#each setupProviders as setup (setup.provider.id)}
    <ModelSelectCard
      provider={setup.provider}
      config={setup.config}
      {onSelect}
      selected={selectedPair?.providerId === setup.provider.id}
      modelId={selectedPair?.providerId === setup.provider.id
        ? selectedPair.model
        : null}
    />
  {/each}
</div>

<script lang="ts">
    interface Props {
        diff: string | number
        style?: string
        value: string | number
        hasSoftDiff?: boolean | undefined
        monospaced?: boolean
        hoverElementOffset?: string
        id?: string | undefined
        hoverValue?: import('svelte').Snippet
    }

    let {
        diff,
        style = '',
        value,
        hasSoftDiff = undefined,
        monospaced = false,
        hoverElementOffset = '-1.1rem',
        id = undefined,
        hoverValue
    }: Props = $props()
</script>

<div
    class:modified={diff !== value}
    class:softDiff={hasSoftDiff === true}
    class="tooltip-base"
    {id}
    class:monospaced
    {style}
>
    {value}
    <div class="hover-element" style={`top: ${hoverElementOffset}`}>
        {#if hoverValue}
            <div class:monospaced>
                {@render hoverValue?.()}
            </div>
        {/if}
        {#if diff !== value}
            <div class="old-value" class:monospaced>
                {diff}
            </div>
        {/if}
    </div>
</div>

<style lang="scss">
    .tooltip-base {
        display: flex;
        align-items: center;
        border-radius: 0.2rem;
        text-align: center;
        justify-content: center;
        cursor: default;
    }

    .softDiff {
        background-color: var(--primary);
        color: var(--primary-text);
    }

    .hover-element {
        display: none;
        flex-direction: column;
        min-width: 100%;
        background-color: var(--tertiary);
        color: var(--tertiary-text);
        border-radius: 0.2rem;
        position: absolute;
        cursor: text;
        box-shadow: rgba(0, 0, 0, 0.4) 0px 2px 2px;
        padding: 0.2rem;
    }

    .old-value {
        color: var(--accent);
    }

    .monospaced {
        font-family: monospace;
    }

    .tooltip-base:hover {
        filter: brightness(1.1);
    }

    .tooltip-base:hover .hover-element {
        display: flex;
    }

    .modified {
        background-color: var(--accent);
        color: var(--accent-text);
        cursor: pointer;
    }
</style>

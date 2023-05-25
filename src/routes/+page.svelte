<script lang="ts">
  import "../app.scss";
  import Highlight from "svelte-highlight";
  import scss from "svelte-highlight/languages/scss";
  import github from "svelte-highlight/styles/github-dark-dimmed";

  const syntax = `@mixin apply-typing(
    $instructions,
    $duration,
    $delay,
    $typing-timing-func: ease-in-out,
    $cursor: true,
    $cursor-thickness: 2px,
    $cursor-style: solid,
    $cursor-color: black,
    $cursor-duration: 1s,
    $cursor-timing-func: ease-in-out,
    $cursor-start-delay: 0s,
    $cursor-end-delay: 2,
    $preserve-whitespace: true,
    $set-content: true
)`;

  const examples = `/* Main heading */
@include typing.apply-typing(
    ("+Want a cool effect?", "s15", "-w1", "s10", "+animation?"),
    6s,
    1s,
    $cursor-color: colors.$text
);


/* Documentation heading */
@include typing.apply-typing(
    ("Docs", "s4", "-1", "s2", "+umentation"),
    2s,
    1s,
    $cursor-color: colors.$text,
    $set-content: false
);`;
</script>

<svelte:head>
  {@html github}
</svelte:head>

<div class="panel intro">
  <h1 class="typing-area typing-area1">
    <span>Want a cool animation?</span>
  </h1>
  <p class="typing-area typing-area2">
    <span>Use this mixin!</span>
  </p>
  <p class="typing-area typing-area3">
    <span>Scroll for documentation</span>
  </p>
</div>

<div class="panel docs">
  <h2 class="typing-area visible typing-area4">
    <span>Documentation</span>
  </h2>

  <div class="docs-content">
    <h3 class="syntax-heading">Syntax:</h3>
    <Highlight langtag class="syntax" language={scss} code={syntax} />

    <h3 class="explanation-heading">Syntax:</h3>
    <div class="explanation">
      <p><code>$instructions</code>: an array of instructions:</p>
      <ul>
        <li><code>[string]</code> = replace content with string</li>
        <li><code>+[string]</code> = add chars one by one</li>
        <li>
          <code>-</code> = remove chars one by one:
          <ul>
            <li><code>-a</code> = remove all</li>
            <li>
              <code>-w[num]</code> = remove number of words (leaving a trailing space)
            </li>
            <li><code>-[num]</code> = remove number of chars</li>
          </ul>
        </li>
        <li><code>s</code> = stay (pause) a number of steps</li>
        <li>use <code>\</code> to escape</li>
      </ul>
      <p><code>$duration</code>: the duration of the animation</p>
      <p><code>$delay</code>: the delay of the animation</p>
      <p>
        <code>$typing-timing-func</code>: the timing function of the animation
      </p>
      <p><code>$cursor</code>: boolean; if a cursor should be present</p>
      <p><code>$cursor-thickness</code>: thickness of the cursor</p>
      <p><code>$cursor-style</code>: same as border-style</p>
      <p><code>$cursor-color</code>: same as border-color</p>
      <p>
        <code>$cursor-duration</code>: duration one cycle of the cursor
        animation
      </p>
      <p>
        <code>$cursor-timing-func</code>: the timing function of the cursor's
        animation
      </p>
      <p><code>$cursor-start-delay</code>: delay of the cursor's animation</p>
      <p>
        <code>$cursor-end-delay</code>: iteration cound; how much iterations
        should the cursor be running after the typing animation is finished
      </p>
      <p>
        <code>$preserve-whitespace</code>: boolean; if whitespace should be
        preserved
      </p>
      <p>
        <code>$set-content</code>: set to false if content is already set in the
        before tag (as in the docs heading example)
      </p>
    </div>

    <h3 class="examples-heading">Examples:</h3>
    <Highlight langtag class="examples" language={scss} code={examples} />
  </div>
</div>

<style lang="scss">
  @use "../sass/typing/typing.scss";
  @use "../sass/colors.scss";

  .panel {
    width: 100%;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .intro {
    gap: 0.75rem;
    justify-content: center;
    position: relative;
  }

  .typing-area * {
    display: none;
  }

  .typing-area1 {
    font-size: 5rem;
    font-weight: bold;

    @include typing.apply-typing(
      ("+Want a cool effect?", "s15", "-w1", "s10", "+animation?"),
      6s,
      1s,
      $cursor-color: colors.$text
    );
  }

  .typing-area2 {
    font-size: 3rem;

    @include typing.apply-typing(
      ("+Use this mixin.", "s7", "-1", "s3", "+!"),
      2s,
      11s,
      $cursor-color: colors.$text,
      $cursor-start-delay: 10s
    );
  }

  .typing-area3 {
    position: absolute;
    bottom: 2rem;
    left: 2rem;

    @include typing.apply-typing(
      ("+Scroll for documentation"),
      4s,
      16s,
      $cursor-color: colors.$text,
      $cursor-start-delay: 14s
    );
  }

  .docs {
    padding: 4.5rem;
    gap: 2.5rem;

    :global(pre code) {
      border-radius: 0.5rem;
      line-height: 1.6;
      padding: 2rem 3rem;
      height: 100%;
    }

    :global(pre) {
      width: 100%;
    }

    :global(ul) {
      list-style-type: disc;
      margin-left: 1.5rem;
    }
  }

  .docs-content {
    display: grid;
    grid-template-areas:
      "syntax-heading explanation-heading examples-heading"
      "syntax explanation examples";
    grid-template-columns: repeat(3, 1fr);
    gap: 2rem;
    justify-items: center;

    h3 {
      font-size: 2rem;
    }
  }

  .syntax-heading {
    grid-area: syntax-heading;
  }

  :global(.syntax) {
    grid-area: syntax;
  }

  .explanation-heading {
    grid-area: explanation-heading;
  }

  .explanation {
    grid-area: explanation;
    padding: 2rem 3rem;
    width: 100%;
    height: 100%;
    background-color: #22272e;
    border-radius: 0.5rem;
    line-height: 1.6;

    :global(code) {
      border-radius: 0.375rem;
      background-color: #4b5057;
      padding: 0.125rem;
    }
  }

  .examples-heading {
    grid-area: examples-heading;
  }

  :global(.examples) {
    grid-area: examples;
  }

  .typing-area4 {
    font-size: 3rem;

    &::before {
      content: "Docs";
    }

    &.visible {
      @include typing.apply-typing(
        ("Docs", "s4", "-1", "s2", "+umentation"),
        2s,
        1s,
        $cursor-color: colors.$text,
        $set-content: false
      );
    }
  }
</style>

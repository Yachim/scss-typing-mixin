<script lang="ts">
  import "../app.scss";
</script>

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
</div>

<style lang="scss">
  @use "../sass/typing/typing.scss";
  @use "../sass/colors.scss";

  .panel {
    width: 100%;
    height: 100vh;
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

<h1 class="typing-area">
  <span>
    Want a cool animation?<br />
    Use this mixin!
  </span>
</h1>

<style lang="scss">
  @use "../sass/typing/typing.scss";

  .typing-area {
    text-align: center;

    span {
      display: none;
    }

    @include typing.apply-typing(
      (
        "+Want a cool effect?",
        "s20",
        "-w1",
        "s10",
        "+animation?",
        "s20",
        "-a",
        "s10",
        "+Use this mixin!"
      ),
      10s,
      1s
    );
  }
</style>

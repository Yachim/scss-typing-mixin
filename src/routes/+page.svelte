<h1 class="typing-area">
  <span>
    Want a cool animation?<br />
    Use this mixin!
  </span>
</h1>

<style lang="scss">
  @use "../sass/typing.scss";

  .typing-area {
    span {
      display: none;
    }

    @include typing.apply-typing(
      [ "",
      "+Want a cool effect?",
      "s20",
      "-7",
      "s10",
      "+animation?",
      "s20",
      "-29",
      "+Use this mixin!" ],
      10s,
      1s
    );
  }
</style>

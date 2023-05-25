<span
  class="typing-area"
  aria-label="Use this mixin to create typing animation."
/>

<style lang="scss">
  @use "sass:list";
  @use "sass:string";
  @use "sass:math";
  @use "sass:map";

  @function to-number($str) {
    $numbers: (
      "0": 0,
      "1": 1,
      "2": 2,
      "3": 3,
      "4": 4,
      "5": 5,
      "6": 6,
      "7": 7,
      "8": 8,
      "9": 9,
    );

    @if string.slice($str, 1, 1) == "-" {
      @error "Only positive integers";
    }

    $num: 0;
    $digits: math.pow(10, string.length($str) - 1);
    @for $char-i from 1 through string.length($str) {
      $char: string.slice($str, $char-i, $char-i);
      @if not map.has-key($numbers, $char) {
        @error "Invalid key: " + $char;
      }

      $num: $num + $digits * map.get($numbers, $char);
      $digits: math.div($digits, 10);
    }

    @return $num;
  }

  @mixin generate-typing($texts) {
    $content: list.nth($texts, 1);
    0% {
      content: $content;
    }

    // -1 because the last is 100%
    $cnt: string.length(list.nth($texts, 1)) - 1;
    @for $text-i from 2 through list.length($texts) {
      $text: list.nth($texts, $text-i);
      $op: string.slice($text, 1, 1);

      @if $op == "+" {
        $cnt: $cnt + string.length($text) - 1;
      } @else if $op == "-" or $op == "s" {
        $num: to-number(string.slice($text, 2));
        $cnt: $cnt + $num;
      } @else {
        @error "The first character of a string should be an operator. Either '+', '-' or 's'";
      }
    }

    $percent-change: math.div(100, $cnt);
    $percent: $percent-change;

    @for $text-i from 2 through list.length($texts) {
      $text: list.nth($texts, $text-i);
      $op: string.slice($text, 1, 1);

      @if $op == "+" {
        @for $char-i from 2 through string.length($text) {
          $char: string.slice($text, $char-i, $char-i);
          $content: $content + $char;

          #{$percent}% {
            content: $content;
          }

          $percent: $percent + $percent-change;
        }
      } @else if $op == "-" {
        $num: to-number(string.slice($text, 2));
        @for $_ from 1 through $num {
          $content: string.slice($content, 1, -2);

          #{$percent}% {
            content: $content;
          }

          $percent: $percent + $percent-change;
        }
      } @else if $op == "s" {
        $num: to-number(string.slice($text, 2));
        @debug $num;
        @for $_ from 1 through $num {
          #{$percent}% {
            content: $content;
          }
          $percent: $percent + $percent-change;
        }
      } @else {
        @error "The first character of a string should be an operator. Either '+', '-' or 's'";
      }
    }

    100% {
      content: $content;
    }
  }

  $blink-cursor-keyframes-name: "blink-cursor-" + string.unique-id();
  @keyframes #{$blink-cursor-keyframes-name} {
    0%,
    49% {
      opacity: 1;
    }
    50%,
    100% {
      opacity: 0;
    }
  }

  /// Remove the unit of a length
  /// @param {Number} $number - Number to remove unit from
  /// @return {Number} - Unitless number
  @function strip-unit($number) {
    @if type-of($number) == "number" and not unitless($number) {
      @return $number / ($number * 0 + 1);
    }

    @return $number;
  }

  // instructions:
  // - first is initial string
  // - `+` = add chars
  // - `-` = remove amout of chars
  // - `s` = stay a number of steps
  @mixin apply-typing(
    $instructions,
    $duration,
    $delay,
    $timing-func,
    $cursor-end-delay: 1
  ) {
    $typing-keyframes-name: "typing-" + string.unique-id();
    @keyframes #{$typing-keyframes-name} {
      @include generate-typing($instructions);
    }

    &::before {
      content: "";
      animation: #{$typing-keyframes-name} $duration $timing-func $delay forwards;
    }

    &::after {
      content: "";
      border-right: 2px solid black;
      animation: #{$blink-cursor-keyframes-name} 1s ease-in-out #{strip-unit(
          $duration + $cursor-end-delay
        )};
    }
  }

  .typing-area {
    @include apply-typing(
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
      1s,
      ease-in-out
    );
  }
</style>

# Advanced information for regular expression matches

ECMAScript proposal, specs and tests for advanced regular expression matches information.

## Rationale

ECMAScript allows to match a regular expression against a string, which returns the matched chunks. For further processing you sometimes additionally require the offsets of the capturing groups of those matches, which are not exposed by the current APIs.

## Proposed solution

### Extend the return value of `RegExp.prototype.exec()` to include the capturing groups offsets

The array returned by [`RegExp.prototype.exec()`](http://www.ecma-international.org/ecma-262/6.0/#sec-regexp.prototype.exec) should include an `offsets` property, which represents an array of the start indices of the match and all the captured groups.

#### Examples

##### Non-global regular expression

```javascript
let matches = /a(b(c))/.exec("abcabc");
```

Then `matches` will contain this:

```javascript
{
  0: "abc",
  1: "bc",
  2: "c",
  index: 0,
  input: "abcabc",
  offsets: [
    0,
    1
    2
  ]
}
```

##### Global regular expression

```javascript
let matches = /a(b(c))/g.exec("abcabc");
```

Then `matches` will contain this when executed the first time:

```javascript
{
  0: "abc",
  1: "bc",
  2: "c",
  index: 0,
  input: "abcabc",
  offsets: [
    0,
    1
    2
  ]
}
```

And this when executed the second time:

```javascript
{
  0: "abc",
  1: "bc",
  2: "c",
  index: 3,
  input: "abcabc",
  offsets: [
    3,
    4
    5
  ]
}
```

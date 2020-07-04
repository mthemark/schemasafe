# Error handling

There are two possible types of errors:
 * Schema compilation (i.e. build-time) errors,
 * Validation (i.e. run-time) errors.

Schema compilation errors always throw synchronously to ensure that no invalid schemas get compiled
and/or produce invalid validation code.

### Validation errors are opt-in

Error reporting is disabled by default and is provided as an opt-in, because users who need to
consume errors would enable that, and users who don't would get more concise generated code and
higher performance this way.

### Options

The options relevant to error reporting are:

  * `includeErrors` — return an array of encountered errors as `errors` property of the validator
    function.\
    E.g.: `validate({}); console.log(validate.errors)`.\
    Note that without `allErrors` option, only the first encountered error is returned.

  * `allErrors` — list all encountered errors, not just the first one. Requires `includeErrors`.

    To ensure that this is not a [DoS vector](./Complexity-checks.md), `pattern`, `format` and
    `uniqueItems` checks will be still skipped if the same exact data property already failed
    other restrictions (and already caused an error which will be included), and `propertyPatterns`
    will be skipped if the parent data object (containing the property) failed other restrictions.\
    That does not affect the result of validation, just the list of reported errors in those cases.

  * `verboseErrors` — include more information in each error object. Requires `includeErrors`.

All of those are opt-ins (i.e. `false` by default).

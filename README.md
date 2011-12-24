Parser Parsetransform
====================

This parse transform makes it write simple parsers.

Simply define functions of the form:

    Fun(Args1,Input,Args2) -> parser:binary(Input);
    Fun(Args1,<<Before,Pat1,After>>,Args2) -> do_stuff;
    ...
    Fun(Args1,<<Before,PatN,After>>,Args2) -> do_stuff;
    Fun(Args1,NoMatch,Args2) -> do_other_stuff;

where `Pat1`..`PatN` are Strings or lists of strings wich must be matched exactly,
the be rewritten to pattern match on the input in a manar similar to `binary:match/2`.


If no clause matches `{error,eof}` is returned, particularly if no final default clause matches.

Notes
-----
* The generated code is reasonably fast, at the expense of code size (particularly `do_stuff` may be duplicated)

* Currently matching only works on binaries, but the plan is to support matching on lists and iolists.

* Matching has been somewhat optimized using tries, specifically if multiple patterns share a common prefix this is matched out first and then dispatched to the appropriate clause.

* Several internal functions are generated of the form `Fun#something`.

Todos
-----
* Reduce code duplication.
* Support matching on iolists and lists.



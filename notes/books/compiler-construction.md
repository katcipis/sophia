# Compiler Construction

These notes are based on the
[Compiler Construction](http://www.ethoberon.ethz.ch/WirthPubl/CBEAll.pdf) book.

As usual the notes are not a smaller version of the book, so there will be no
explicit explanation on how to build compilers, just some remarks of the things
that got my attention (the basics I got from colleague).

If it was a completely different approach on how to build compilers, perhaps it would
make sense, but it is prettu much standard LL/LR parsing theory with lexers/scanners
involved. It is more to review stuff and to find some nice subtleties (and some where found).

# Attributed Grammars and Semantics 

This is the chapter 5 of the book and it presents a very interesting idea, which is
to add attributes to certain gramatical constructs. The idea is hard to understand until
you get a more concrete example, as type checking. Quoting directly:

```
Rules  about  type  compatibility  are  indeed  also  static  in  the  sense 
that  they  can  be  verified  without execution of the program.

Hence, their separation from purely syntactic rules app
ears quite arbitrary, and their  integration  into  the  syntax  in 
the  form  of  attribute  rules  is  entirely  appropriate.

However,  we  note that  attributed  grammars  obtain  a  new  dimension, 
if  the  possible  attribute  values  (here,  types)  and  their  
number are not known a priori. 
```

That got me thinking that type checking is validated at the syntatic level, but
grammars and parser generators usually ignore this and type checking is done
implicitely inside the evaluator (the AST is usually built and them it is
checked for type mismatches). The types could be checked by the same code
that builds the AST, and parser generators could also handle it generically
if a proper syntax is added on something like EBNF.

This is very new to me and I'm not sure if the syntax proposed for this on the
book is the best one, but the higher level idea do make sense until experience
proves otherwise.

# Handling syntatic errors

As the book evolves on the implementation of a compiler for the language Oberon-0
it gets on the point of how to handle errors on the parsing process:

```
As  soon  as  an  inacceptable  symbol  turns  up,
the  task  of  the  parser  is  completed,  and  the  
process  of  syntax  analysis  is  terminated.

For  practical  applications,  however,  this  proposition  is  
unacceptable. A genuine compiler must indicate an error diagnostic mess
age and thereafter proceed with the  analysis.  
```

Sadly the book does not evolve on **why** a genuine compiler must
proceed the analysis. If you have several errors and every time you find one
the compiler stops the only overhead is recompiling, you will need to analyze
and fix each error anyway. It seems to be that this **genuine** property is
related more to efficiency than to something conceptual. A compiler that
fails on the first found error is much simpler and the first error is
the only one that is true, all subsequent errors depend on heuristics
that can easily be wrong.

You have to assume the error and skips parts of the text to continue parsing, which
is a process that can get pretty complicated and when it fails (and it will since it
is a heuristics) will only produce a list of fucked up errors.

Defining good heuristics even depends on understanding human cognition,
as is said on the book:

```
The  technique  of  choosing  good  hypotheses  is  complicated.
It  ultimately  rests  upon  heuristics,  as  the problem has so far eluded
formal treatment.

The principal reason for this is that the formal syntax ignores 
factors which are essential for the human recognition 
of a sentence. For instance, a missing punctuation symbol
is a frequent mistake, not only in program texts, but an operator symbol
is seldom omitted in an arithmetic  expression.
```

It is a great example, punctuation is more often ignored by humans than operations.
If you are adding, "+" is fundamental, while ";" never is, it is just a gramatical
formalism required by the compiler to understand things. Humans do fine with just newlines
for example, which brings the subject of automatically adding semicolons or even
spaces/tabs with semantic values as is Python.

The problem is then summarized:

```
To summarize, we postulate 
the following quality criteria for error handling:

1. As many errors as possible must be detected in a single scan through the text. 
2. As few additional assumptions as possible about the language are to be made. 
3. Error handling features should not slow down the parser appreciably. 
4. The parser program should not grow in size significantly.
```

It is not a simple problem and it has clear opposing forces (almost all interesting problems do),
like having as many errors as possible AND not growing the parser significantly. The book
does provide some nice ideas on how to handle this, even using syntax constructions
to help identify how much text must be skipped, but to be honest it seems that
with the growth of computational power and other techniques that aid efficient
re-compilation it seems that this problem could be solved by not creating it.
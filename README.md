Branch of [Gramtest](https://github.com/codelion/gramtest)

# Purpose
Gramtest uses ANTLR to parse E/BNF grammar files, then generate test cases that correspond to the supplied grammar.
The problem is that it takes a recursive approach and will generate every possible combination of grammars in order.
Using the ``mingen`` flag allows for some randomization, but it generally falls on its face.

TLDR; If you need to generate randomized BNF grammar test cases, you'll be wanting this project.

# Differences
* Removes "bug" where using grammar to pick one and only one alternative would result in more than one option being
picked anyway
* Shuffles grammar test cases as the grammar is parsed. This ensures a randomized result (NOTE: -mingen must be false)
* Adds a printTests() method in extractor
* Adds support for parsing newline characters ("_n" in your grammar file)
* Default options are now maxNum = 1, depth = 2, mingen = false

# Additional Usage
You can compile this branch to a jar and use it the same way as gramtest, or add the project as a dependency and do
the following:

~~~ java
          Lexer lexer = new bnfLexer(new ANTLRFileStream(filename));
          CommonTokenStream tokens = new CommonTokenStream(lexer);
          bnfParser grammarParser = new bnfParser(tokens);
          ParserRuleContext tree = grammarparser.rulelist();
          GeneratorVisitor extractor = new GeneratorVisitor(max, depth, useMinGen);
          extractor.visit(tree);
~~~

Calling ``extractor.visit()`` will generate the appropriate test cases. They can be accessed using the getter, or the
 print method I've added (printTests()).
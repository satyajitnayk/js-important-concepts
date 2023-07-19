In Handlebars, there are several types of statements and literals that you can use to build your templates. Let's go
through each of them with detailed explanations and examples:

1. MustacheStatement:
   The MustacheStatement is used to output the value of a variable or an expression within double curly
   braces (`{{ }}`). It is the most common statement in Handlebars.

   Example:
   ```
   {{name}}
   ```

2. BlockStatement:
   The BlockStatement is used to create control flow structures such as conditionals and loops. It starts with a pound
   sign (`#`) followed by the keyword for the specific block.

   Example (if statement):
   ```
   {{#if condition}}
     <p>The condition is true.</p>
   {{else}}
     <p>The condition is false.</p>
   {{/if}}
   ```

3. SubExpression:
   The SubExpression is a statement used to invoke helper functions or perform more complex operations within double
   parentheses (`( )`).

   Example (capitalize helper function):
   ```
   {{capitalize (concat firstName " " lastName)}}
   ```

4. PathExpression:
- The PathExpression is used to access the value of a property or key within an object or context. It can be a simple
   identifier or a nested path separated by dots (`.`). The `person.name` is a `PathExpression`. It represents the path 
   to access the value of the name property within the person object or context.

- Path expressions are used to navigate through the object hierarchy or context and access specific properties or 
   keys. They consist of one or more identifiers separated by dots (.). Each identifier represents a property or key 
   within the object or context.

   Example:
   ```
   {{person.name}}
   ```

5. StringLiteral:
   The StringLiteral represents a string value enclosed in double quotes (`"`).

   Example:
   ```
   {{sayHello "John"}}
   ```

6. BooleanLiteral:
   The BooleanLiteral represents a boolean value, either `true` or `false`.

   Example:
   ```
   {{#if isReady}}
     <p>The system is ready.</p>
   {{/if}}
   ```

7. NumberLiteral:
   The NumberLiteral represents a numeric value.

   Example:
   ```
   {{multiply 2 3}}
   ```

8. UndefinedLiteral:
   The UndefinedLiteral represents an undefined value.

   Example:
   ```
   {{#if variable}}
     <p>The variable is defined.</p>
   {{else}}
     <p>The variable is undefined.</p>
   {{/if}}
   ```

9. NullLiteral:
   The NullLiteral represents a null value.

   Example:
   ```
   {{#unless variable}}
     <p>The variable is null.</p>
   {{/unless}}
   ```

These are the basic elements in Handlebars templates. You can combine them to create powerful and dynamic templates for
your applications.

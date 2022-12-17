---
layout: default
title: Operators / Symbols
parent: Rust
grand_parent: Notes
---

<h2 id="operators-symbols">Operators / Symbols</h2>
<p>Contains all possible operators and symbols that have a predefined functionality.</p>
<h3 id="operators">Operators</h3>
<p>If a trait is overloadable the relevant trait to use, to overload that operator is listed.</p>
<table>
<thead>
<tr>
<th>Operator</th>
<th>Example</th>
<th>Explanation</th>
<th>Overloadable?</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>!</code></td>
<td><code>ident!(...)</code>, <code>ident!{...}</code>, <code>ident![...]</code></td>
<td>Macro expansion</td>
<td></td>
</tr>
<tr>
<td><code>!</code></td>
<td><code>!expr</code></td>
<td>Bitwise or logical complement</td>
<td><code>Not</code></td>
</tr>
<tr>
<td><code>!=</code></td>
<td><code>expr != expr</code></td>
<td>Nonequality comparison</td>
<td><code>PartialEq</code></td>
</tr>
<tr>
<td><code>%</code></td>
<td><code>expr % expr</code></td>
<td>Arithmetic remainder</td>
<td><code>Rem</code></td>
</tr>
<tr>
<td><code>%=</code></td>
<td><code>var %= expr</code></td>
<td>Arithmetic remainder and assignment</td>
<td><code>RemAssign</code></td>
</tr>
<tr>
<td><code>&amp;</code></td>
<td><code>&amp;expr</code>, <code>&amp;mut expr</code></td>
<td>Borrow</td>
<td></td>
</tr>
<tr>
<td><code>&amp;</code></td>
<td><code>&amp;type</code>, <code>&amp;mut type</code>, <code>&amp;&#39;a type</code>, <code>&amp;&#39;a mut type</code></td>
<td>Borrowed pointer type</td>
<td></td>
</tr>
<tr>
<td><code>&amp;</code></td>
<td><code>expr &amp; expr</code></td>
<td>Bitwise AND</td>
<td><code>BitAnd</code></td>
</tr>
<tr>
<td><code>&amp;=</code></td>
<td><code>var &amp;= expr</code></td>
<td>Bitwise AND and assignment</td>
<td><code>BitAndAssign</code></td>
</tr>
<tr>
<td><code>&amp;&amp;</code></td>
<td><code>expr &amp;&amp; expr</code></td>
<td>Short-circuiting logical AND</td>
<td></td>
</tr>
<tr>
<td><code>*</code></td>
<td><code>expr * expr</code></td>
<td>Arithmetic multiplication</td>
<td><code>Mul</code></td>
</tr>
<tr>
<td><code>*=</code></td>
<td><code>var *= expr</code></td>
<td>Arithmetic multiplication and assignment</td>
<td><code>MulAssign</code></td>
</tr>
<tr>
<td><code>*</code></td>
<td><code>*expr</code></td>
<td>Dereference</td>
<td><code>Deref</code></td>
</tr>
<tr>
<td><code>*</code></td>
<td><code>*const type</code>, <code>*mut type</code></td>
<td>Raw pointer</td>
<td></td>
</tr>
<tr>
<td><code>+</code></td>
<td><code>trait + trait</code>, <code>&#39;a + trait</code></td>
<td>Compound type constraint</td>
<td></td>
</tr>
<tr>
<td><code>+</code></td>
<td><code>expr + expr</code></td>
<td>Arithmetic addition</td>
<td><code>Add</code></td>
</tr>
<tr>
<td><code>+=</code></td>
<td><code>var += expr</code></td>
<td>Arithmetic addition and assignment</td>
<td><code>AddAssign</code></td>
</tr>
<tr>
<td><code>,</code></td>
<td><code>expr, expr</code></td>
<td>Argument and element separator</td>
<td></td>
</tr>
<tr>
<td><code>-</code></td>
<td><code>- expr</code></td>
<td>Arithmetic negation</td>
<td><code>Neg</code></td>
</tr>
<tr>
<td><code>-</code></td>
<td><code>expr - expr</code></td>
<td>Arithmetic subtraction</td>
<td><code>Sub</code></td>
</tr>
<tr>
<td><code>-=</code></td>
<td><code>var -= expr</code></td>
<td>Arithmetic subtraction and assignment</td>
<td><code>SubAssign</code></td>
</tr>
<tr>
<td><code>-&gt;</code></td>
<td><code>fn(...) -&gt; type</code>, <code>&vert;...&vert; -&gt; type</code></td>
<td>Function and closure return type</td>
<td></td>
</tr>
<tr>
<td><code>.</code></td>
<td><code>expr.ident</code></td>
<td>Member access</td>
<td></td>
</tr>
<tr>
<td><code>..</code></td>
<td><code>..</code>, <code>expr..</code>, <code>..expr</code>, <code>expr..expr</code></td>
<td>Right-exclusive range literal</td>
<td><code>PartialOrd</code></td>
</tr>
<tr>
<td><code>..=</code></td>
<td><code>..=expr</code>, <code>expr..=expr</code></td>
<td>Right-inclusive range literal</td>
<td><code>PartialOrd</code></td>
</tr>
<tr>
<td><code>..</code></td>
<td><code>..expr</code></td>
<td>Struct literal update syntax</td>
<td></td>
</tr>
<tr>
<td><code>..</code></td>
<td><code>variant(x, ..)</code>, <code>struct_type { x, .. }</code></td>
<td>“And the rest” pattern binding</td>
<td></td>
</tr>
<tr>
<td><code>...</code></td>
<td><code>expr...expr</code></td>
<td>(Deprecated, use <code>..=</code> instead) In a pattern: inclusive range pattern</td>
<td></td>
</tr>
<tr>
<td><code>/</code></td>
<td><code>expr / expr</code></td>
<td>Arithmetic division</td>
<td><code>Div</code></td>
</tr>
<tr>
<td><code>/=</code></td>
<td><code>var /= expr</code></td>
<td>Arithmetic division and assignment</td>
<td><code>DivAssign</code></td>
</tr>
<tr>
<td><code>:</code></td>
<td><code>pat: type</code>, <code>ident: type</code></td>
<td>Constraints</td>
<td></td>
</tr>
<tr>
<td><code>:</code></td>
<td><code>ident: expr</code></td>
<td>Struct field initializer</td>
<td></td>
</tr>
<tr>
<td><code>:</code></td>
<td><code>&#39;a: loop {...}</code></td>
<td>Loop label</td>
<td></td>
</tr>
<tr>
<td><code>;</code></td>
<td><code>expr;</code></td>
<td>Statement and item terminator</td>
<td></td>
</tr>
<tr>
<td><code>;</code></td>
<td><code>[...; len]</code></td>
<td>Part of fixed-size array syntax</td>
<td></td>
</tr>
<tr>
<td><code>&lt;&lt;</code></td>
<td><code>expr &lt;&lt; expr</code></td>
<td>Left-shift</td>
<td><code>Shl</code></td>
</tr>
<tr>
<td><code>&lt;&lt;=</code></td>
<td><code>var &lt;&lt;= expr</code></td>
<td>Left-shift and assignment</td>
<td><code>ShlAssign</code></td>
</tr>
<tr>
<td><code>&lt;</code></td>
<td><code>expr &lt; expr</code></td>
<td>Less than comparison</td>
<td><code>PartialOrd</code></td>
</tr>
<tr>
<td><code>&lt;=</code></td>
<td><code>expr &lt;= expr</code></td>
<td>Less than or equal to comparison</td>
<td><code>PartialOrd</code></td>
</tr>
<tr>
<td><code>=</code></td>
<td><code>var = expr</code>, <code>ident = type</code></td>
<td>Assignment/equivalence</td>
<td></td>
</tr>
<tr>
<td><code>==</code></td>
<td><code>expr == expr</code></td>
<td>Equality comparison</td>
<td><code>PartialEq</code></td>
</tr>
<tr>
<td><code>=&gt;</code></td>
<td><code>pat =&gt; expr</code></td>
<td>Part of match arm syntax</td>
<td></td>
</tr>
<tr>
<td><code>&gt;</code></td>
<td><code>expr &gt; expr</code></td>
<td>Greater than comparison</td>
<td><code>PartialOrd</code></td>
</tr>
<tr>
<td><code>&gt;=</code></td>
<td><code>expr &gt;= expr</code></td>
<td>Greater than or equal to comparison</td>
<td><code>PartialOrd</code></td>
</tr>
<tr>
<td><code>&gt;&gt;</code></td>
<td><code>expr &gt;&gt; expr</code></td>
<td>Right-shift</td>
<td><code>Shr</code></td>
</tr>
<tr>
<td><code>&gt;&gt;=</code></td>
<td><code>var &gt;&gt;= expr</code></td>
<td>Right-shift and assignment</td>
<td><code>ShrAssign</code></td>
</tr>
<tr>
<td><code>@</code></td>
<td><code>ident @ pat</code></td>
<td>Pattern binding</td>
<td></td>
</tr>
<tr>
<td><code>^</code></td>
<td><code>expr ^ expr</code></td>
<td>Bitwise exclusive OR</td>
<td><code>BitXor</code></td>
</tr>
<tr>
<td><code>^=</code></td>
<td><code>var ^= expr</code></td>
<td>Bitwise exclusive OR and assignment</td>
<td><code>BitXorAssign</code></td>
</tr>
<tr>
<td><code>&vert;</code></td>
<td><code>pat &vert; pat</code></td>
<td>Pattern alternatives</td>
<td></td>
</tr>
<tr>
<td><code>&vert;</code></td>
<td><code>expr &vert; expr</code></td>
<td>Bitwise OR</td>
<td><code>BitOr</code></td>
</tr>
<tr>
<td><code>&vert;=</code></td>
<td><code>var &vert;= expr</code></td>
<td>Bitwise OR and assignment</td>
<td><code>BitOrAssign</code></td>
</tr>
<tr>
<td><code>&vert;&vert;</code></td>
<td><code>expr &vert;&vert; expr</code></td>
<td>Short-circuiting logical OR</td>
<td></td>
</tr>
<tr>
<td><code>?</code></td>
<td><code>expr?</code></td>
<td>Error propagation</td>
</tr>
</tbody>
</table>
<h3 id="non-operator-symbols">Non-operator Symbols</h3>
<p>Symbols that don&#39;t function as operators (they don&#39;t behave like a function or method call). </p>
<table>
<thead>
<tr>
<th>Symbol</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>&#39;ident</code></td>
<td>Named lifetime or loop label</td>
</tr>
<tr>
<td><code>...u8</code>, <code>...i32</code>, <code>...f64</code>, <code>...usize</code>, etc.</td>
<td>Numeric literal of specific type</td>
</tr>
<tr>
<td><code>&quot;...&quot;</code></td>
<td>String literal</td>
</tr>
<tr>
<td><code>r&quot;...&quot;</code>, <code>r#&quot;...&quot;#</code>, <code>r##&quot;...&quot;##</code>, etc.</td>
<td>Raw string literal, escape characters not processed</td>
</tr>
<tr>
<td><code>b&quot;...&quot;</code></td>
<td>Byte string literal; constructs an array of bytes instead of a string</td>
</tr>
<tr>
<td><code>br&quot;...&quot;</code>, <code>br#&quot;...&quot;#</code>, <code>br##&quot;...&quot;##</code>, etc.</td>
<td>Raw byte string literal, combination of raw and byte string literal</td>
</tr>
<tr>
<td><code>&#39;...&#39;</code></td>
<td>Character literal</td>
</tr>
<tr>
<td><code>b&#39;...&#39;</code></td>
<td>ASCII byte literal</td>
</tr>
<tr>
<td><code>&vert;...&vert; expr</code></td>
<td>Closure</td>
</tr>
<tr>
<td><code>!</code></td>
<td>Always empty bottom type for diverging functions</td>
</tr>
<tr>
<td><code>_</code></td>
<td>“Ignored” pattern binding; also used to make integer literals readable</td>
</tr>
</tbody>
</table>
<h3 id="item-paths">Item paths</h3>
<table>
<thead>
<tr>
<th>Symbol</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>ident::ident</code></td>
<td>Namespace path</td>
</tr>
<tr>
<td><code>::path</code></td>
<td>Path relative to the crate root (i.e., an explicitly absolute path)</td>
</tr>
<tr>
<td><code>self::path</code></td>
<td>Path relative to the current module (i.e., an explicitly relative path).</td>
</tr>
<tr>
<td><code>super::path</code></td>
<td>Path relative to the parent of the current module</td>
</tr>
<tr>
<td><code>type::ident</code>, <code>&lt;type as trait&gt;::ident</code></td>
<td>Associated constants, functions, and types</td>
</tr>
<tr>
<td><code>&lt;type&gt;::...</code></td>
<td>Associated item for a type that cannot be directly named (e.g., <code>&lt;&amp;T&gt;::...</code>, <code>&lt;[T]&gt;::...</code>, etc.)</td>
</tr>
<tr>
<td><code>trait::method(...)</code></td>
<td>Disambiguating a method call by naming the trait that defines it</td>
</tr>
<tr>
<td><code>type::method(...)</code></td>
<td>Disambiguating a method call by naming the type for which it’s defined</td>
</tr>
<tr>
<td><code>&lt;type as trait&gt;::method(...)</code></td>
<td>Disambiguating a method call by naming the trait and type</td>
</tr>
</tbody>
</table>
<h3 id="generic-type-parameters">Generic type parameters</h3>
<table>
<thead>
<tr>
<th>Symbol</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>path&lt;...&gt;</code></td>
<td>Specifies parameters to generic type in a type (e.g., <code>Vec&lt;u8&gt;</code>)</td>
</tr>
<tr>
<td><code>path::&lt;...&gt;</code>, <code>method::&lt;...&gt;</code></td>
<td>Specifies parameters to generic type, function, or method in an expression; often referred to as turbofish (e.g., <code>&quot;42&quot;.parse::&lt;i32&gt;()</code>)</td>
</tr>
<tr>
<td><code>fn ident&lt;...&gt; ...</code></td>
<td>Define generic function</td>
</tr>
<tr>
<td><code>struct ident&lt;...&gt; ...</code></td>
<td>Define generic structure</td>
</tr>
<tr>
<td><code>enum ident&lt;...&gt; ...</code></td>
<td>Define generic enumeration</td>
</tr>
<tr>
<td><code>impl&lt;...&gt; ...</code></td>
<td>Define generic implementation</td>
</tr>
<tr>
<td><code>for&lt;...&gt; type</code></td>
<td>Higher-ranked lifetime bounds</td>
</tr>
<tr>
<td><code>type&lt;ident=type&gt;</code></td>
<td>A generic type where one or more associated types have specific assignments (e.g., <code>Iterator&lt;Item=T&gt;</code>)</td>
</tr>
</tbody>
</table>
<h3 id="type-bounds">Type bounds</h3>
<table>
<thead>
<tr>
<th>Symbol</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>T: U</code></td>
<td>Generic parameter <code>T</code> constrained to types that implement <code>U</code></td>
</tr>
<tr>
<td><code>T: &#39;a</code></td>
<td>Generic type <code>T</code> must outlive lifetime <code>&#39;a</code> (meaning the type cannot transitively contain any references with lifetimes shorter than <code>&#39;a</code>)</td>
</tr>
<tr>
<td><code>T: &#39;static</code></td>
<td>Generic type <code>T</code> contains no borrowed references other than <code>&#39;static</code> ones</td>
</tr>
<tr>
<td><code>&#39;b: &#39;a</code></td>
<td>Generic lifetime <code>&#39;b</code> must outlive lifetime <code>&#39;a</code></td>
</tr>
<tr>
<td><code>T: ?Sized</code></td>
<td>Allow generic type parameter to be a dynamically sized type</td>
</tr>
<tr>
<td><code>&#39;a + trait</code>, <code>trait + trait</code></td>
<td>Compound type constraint</td>
</tr>
</tbody>
</table>
<h3 id="macros">Macros</h3>
<table>
<thead>
<tr>
<th>Symbol</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>#[meta]</code></td>
<td>Outer attribute</td>
</tr>
<tr>
<td><code>#![meta]</code></td>
<td>Inner attribute</td>
</tr>
<tr>
<td><code>$ident</code></td>
<td>Macro substitution</td>
</tr>
<tr>
<td><code>$ident:kind</code></td>
<td>Macro capture</td>
</tr>
<tr>
<td><code>$(…)…</code></td>
<td>Macro repetition</td>
</tr>
<tr>
<td><code>ident!(...)</code>, <code>ident!{...}</code>, <code>ident![...]</code></td>
<td>Macro invocation</td>
</tr>
</tbody>
</table>
<h3 id="comments">Comments</h3>
<table>
<thead>
<tr>
<th>Symbol</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>//</code></td>
<td>Line comment</td>
</tr>
<tr>
<td><code>//!</code></td>
<td>Inner line doc comment</td>
</tr>
<tr>
<td><code>///</code></td>
<td>Outer line doc comment</td>
</tr>
<tr>
<td><code>/*...*/</code></td>
<td>Block comment</td>
</tr>
<tr>
<td><code>/*!...*/</code></td>
<td>Inner block doc comment</td>
</tr>
<tr>
<td><code>/**...*/</code></td>
<td>Outer block doc comment</td>
</tr>
</tbody>
</table>
<h3 id="tuples">Tuples</h3>
<table>
<thead>
<tr>
<th>Symbol</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>()</code></td>
<td>Empty tuple (aka unit), both literal and type</td>
</tr>
<tr>
<td><code>(expr)</code></td>
<td>Parenthesized expression</td>
</tr>
<tr>
<td><code>(expr,)</code></td>
<td>Single-element tuple expression</td>
</tr>
<tr>
<td><code>(type,)</code></td>
<td>Single-element tuple type</td>
</tr>
<tr>
<td><code>(expr, ...)</code></td>
<td>Tuple expression</td>
</tr>
<tr>
<td><code>(type, ...)</code></td>
<td>Tuple type</td>
</tr>
<tr>
<td><code>expr(expr, ...)</code></td>
<td>Function call expression; also used to initialize tuple <code>struct</code>s and tuple <code>enum</code> variants</td>
</tr>
<tr>
<td><code>expr.0</code>, <code>expr.1</code>, etc.</td>
<td>Tuple indexing</td>
</tr>
</tbody>
</table>
<h3 id="curly-brackets">Curly Brackets</h3>
<table>
<thead>
<tr>
<th>Context</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>{...}</code></td>
<td>Block expression</td>
</tr>
<tr>
<td><code>Type {...}</code></td>
<td><code>struct</code> literal</td>
</tr>
</tbody>
</table>
<h3 id="square-brackets">Square Brackets</h3>
<table>
<thead>
<tr>
<th>Context</th>
<th>Explanation</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>[...]</code></td>
<td>Array literal</td>
</tr>
<tr>
<td><code>[expr; len]</code></td>
<td>Array literal containing <code>len</code> copies of <code>expr</code></td>
</tr>
<tr>
<td><code>[type; len]</code></td>
<td>Array type containing <code>len</code> instances of <code>type</code></td>
</tr>
<tr>
<td><code>expr[expr]</code></td>
<td>Collection indexing. Overloadable (<code>Index</code>, <code>IndexMut</code>)</td>
</tr>
<tr>
<td><code>expr[..]</code>, <code>expr[a..]</code>, <code>expr[..b]</code>, <code>expr[a..b]</code></td>
<td>Collection indexing pretending to be collection slicing, using <code>Range</code>, <code>RangeFrom</code>, <code>RangeTo</code>, or <code>RangeFull</code> as the “index”</td>
</tr>
</tbody>
</table>

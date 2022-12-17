---
layout: default
title: Keywords
parent: Rust
grand_parent: Notes
---

<h2 id="keywords">Keywords</h2>
<p>Contains all keywords currently reserved by Rust and their respective use.</p>
<h3 id="raw-identifiers">Raw identifiers</h3>
<p>Keywords can still be used as function names and so on, when adding r#, to the keyword name both when defining and then when using it. </p>
<pre><code class="lang-rust"><span class="hljs-function"><span class="hljs-keyword">fn</span> <span class="hljs-title">r</span>#<span class="hljs-title">match</span></span>() -&gt; <span class="hljs-keyword">bool</span> { 
    <span class="hljs-keyword">return</span> <span class="hljs-literal">true</span>; 
}

<span class="hljs-function"><span class="hljs-keyword">fn</span> <span class="hljs-title">main</span></span>() { 
    <span class="hljs-built_in">assert!</span>(r#<span class="hljs-keyword">match</span>()); 
}
</code></pre>
<h3 id="keyword-list">Keyword list</h3>
<table>
<thead>
<tr>
<th>Keyword</th>
<th>Use</th>
</tr>
</thead>
<tbody>
<tr>
<td>as</td>
<td>Perform primitive casting, disambiguate the specific trait containing an item, or rename items in use statements</td>
</tr>
<tr>
<td>await</td>
<td>Suspend execution until the result of a Future is ready</td>
</tr>
<tr>
<td>break</td>
<td>Exit a loop immediately</td>
</tr>
<tr>
<td>const</td>
<td>Define constant items or constant raw pointers</td>
</tr>
<tr>
<td>continue</td>
<td>Continue to the next loop iteration</td>
</tr>
<tr>
<td>crate</td>
<td>In a module path, refers to the crate root</td>
</tr>
<tr>
<td>dyn</td>
<td>Dynamic dispatch to a trait object</td>
</tr>
<tr>
<td>else</td>
<td>Fallback for if and if let control flow constructs</td>
</tr>
<tr>
<td>enum</td>
<td>Define an enumeration</td>
</tr>
<tr>
<td>extern</td>
<td>Link an external function or variable</td>
</tr>
<tr>
<td>false</td>
<td>Boolean false literal</td>
</tr>
<tr>
<td>fn</td>
<td>Define a function or the function pointer type</td>
</tr>
<tr>
<td>for</td>
<td>Loop over items from an iterator, implement a trait, or specify a higher-ranked lifetime</td>
</tr>
<tr>
<td>if</td>
<td>Branch based on the result of a conditional expression</td>
</tr>
<tr>
<td>impl</td>
<td>Implement inherent or trait functionality</td>
</tr>
<tr>
<td>in</td>
<td>Part of for loop syntax</td>
</tr>
<tr>
<td>let</td>
<td>Bind a variable</td>
</tr>
<tr>
<td>loop</td>
<td>Loop unconditionally</td>
</tr>
<tr>
<td>match</td>
<td>Match a value to patterns</td>
</tr>
<tr>
<td>mod</td>
<td>Define a module</td>
</tr>
<tr>
<td>move</td>
<td>Make a closure take ownership of all its captures</td>
</tr>
<tr>
<td>mut</td>
<td>Denote mutability in references, raw pointers, or pattern bindings</td>
</tr>
<tr>
<td>pub</td>
<td>Denote public visibility in struct fields, impl blocks, or modules</td>
</tr>
<tr>
<td>ref</td>
<td>Bind by reference</td>
</tr>
<tr>
<td>return</td>
<td>Return from function</td>
</tr>
<tr>
<td>Self</td>
<td>A type alias for the type we are defining or implementing</td>
</tr>
<tr>
<td>self</td>
<td>Method subject or current module</td>
</tr>
<tr>
<td>static</td>
<td>Global variable or lifetime lasting the entire program execution</td>
</tr>
<tr>
<td>struct</td>
<td>Define a structure</td>
</tr>
<tr>
<td>super</td>
<td>Parent module of the current module</td>
</tr>
<tr>
<td>trait</td>
<td>Define a trait</td>
</tr>
<tr>
<td>true</td>
<td>Boolean true literal</td>
</tr>
<tr>
<td>type</td>
<td>Define a type alias or associated type</td>
</tr>
<tr>
<td>union</td>
<td>Define a union; is only a keyword when used in a union declaration</td>
</tr>
<tr>
<td>unsafe</td>
<td>Denote unsafe code, functions, traits, or implementations</td>
</tr>
<tr>
<td>use</td>
<td>Bring symbols into scope</td>
</tr>
<tr>
<td>where</td>
<td>Denote clauses that constrain a type</td>
</tr>
<tr>
<td>while</td>
<td>Loop conditionally based on the result of an expression</td>
</tr>
<tr>
<td>abstract</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>become</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>box</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>do</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>final</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>macro</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>override</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>priv</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>try</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>typeof</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>unsized</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>virtual</td>
<td>No functionality yet</td>
</tr>
<tr>
<td>yield</td>
<td>No functionality yet</td>
</tr>
</tbody>
</table>

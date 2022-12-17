---
layout: default
title: Cargo
parent: Rust
grand_parent: Notes
---

<h2>Cargo</h2>
<h3>Useful Commands</h3>
<html>
<table>
<tr>
    <td>
        <b>COMMAND</b>
    </td>
    <td>
        <b>DESCRIPTION</b>
    </td>
</tr>
<tr>
    <td>
        <code>cargo --version</code>
    </td>
    <td>
        Returns the currently installed version number of rust if installed (comes with cargo preinstalled)
    </td>
</tr>
<tr>
    <td>
        <code>cargo new &ltprojectname&gt</code>
    </td>
    <td>
        Creates a new project and git repo with the given name
        </br>
        <b>Add --vcs=git to not create a new git repo</b>
    </td>
</tr>
<tr>
    <td>
        <code>cargo build</code>
    </td>
    <td>
        Creates an executable file in <b>target/debug/<projectname>.exe</b>
        <br/>
        Run with <b>.\target\debug\&ltprojectname&gt.exe</b>
        <br/>
        To build for release use <b>--release</b>
    </td>
</tr>
<tr>
    <td>
        <code>cargo run</code>
    </td>
    <td>
        Both compiles code and runs the resulting executable
    </td>
</tr>
<tr>
    <td>
        <code>cargo update</code>
    </td>
    <td>
        Searches for newer version of the specified dependencies and updates the <b>Cargo.lock</b> file accordingly
        <br/>
        (To update to the next minor version 0.X.0, the <b>Cargo.lock</b> file has to be changed by hand)
    </td>
</tr>
<tr>
    <td>
        <code>cargo doc --open</code>
    </td>
    <td>
        Locally builds a documentation of all included dependencies and opens them in the browser
    </td>
</tr>
</table>
</html>

<h3>Cargo.toml</h3>
<p>Is the build file for rust and includes the package name, version and edition. As well as optional dependencies and everything that dependency is fetched from [Crates.io](https://crates.io/) and built as well when we build our package.</p>

<pre><code class="lang-toml"><span class="hljs-section">[package]</span>
<span class="hljs-attr">name</span> = <span class="hljs-string">"example_project"</span>
<span class="hljs-attr">version</span> = <span class="hljs-string">"0.1.0"</span>
<span class="hljs-attr">edition</span> = <span class="hljs-string">"2021"</span>
<span class="hljs-comment"># See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html</span>
<span class="hljs-section">[dependencies]</span>
<span class="hljs-attr">rand</span> = <span class="hljs-string">"^0.8.3"</span>
</code></pre>

<p><code>"0.8.3" = "^0.8.3"</code>: All version from <b>0.8.3</b> up to but not including <b>0.9.0</b> (<i>might contain breaking API changes</i>)</p>

<h3>Cargo.lock</h3>
<p>When rebuilding a project, this file will be used to get the saved versions instead of trying to figure out the newest versions again. Meaning the package will stay in the specified version until it is explicitly upgraded. With <code>cargo update</code> explained above.</p>

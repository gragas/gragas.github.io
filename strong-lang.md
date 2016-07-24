---
layout: page
title: Strong Programming Language
permalink: /strong-lang/
---

## Introduction

It seems that it is a coming-of-age tradition for aspiring PL researchers to write their own Lisp. I suppose that I am upholding that tradition, and, like many of my peers, I think I can bring something to the table that's *different* than all the rest—something novel and maybe even useful. Behold, my dearest creation: the **Strong Programming Language**.

I am writing the language in an attempt to research one problem: **metaprogramming**. Specifically, I want to investigate whether it is possible to have the benefits of both a static type system (with much of the language's overheads resolved at compile time) and the ability to easily program the language (to metaprogram). A second constraint I want to impose is the ability to **compile to Rust** and easily interface with C. As described in an articled linked in the next paragraph, achieving these to goals may lead to easy and *good* interop with many other languages.

Many papers, texts, and articles have influenced how I approach the implementation of this language. Extremely influential has been [this article about metaprogramming](http://notes.willcrichton.net/the-coming-age-of-the-polyglot-programmer/) by Will Crichton ([here's his second post in the series](http://notes.willcrichton.net/rust-the-new-llvm/)). You can find a list of all the PL readings that have influenced me [here](/pl-readings), and a list of resources I have used [here](/pl-resources).

## Language Specification

*To be completed.*

## Current State of Stronglang

I'm currently writing a prototype interpreter and REPL in C. I intend to soon implement the language in Rust with full compilation capabilities.
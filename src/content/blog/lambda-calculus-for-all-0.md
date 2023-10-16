---
slug: lambda-calculus-for-all-part-0-creating-booleans-using-functions
title: "Lambda Calculus For All: Part 0 - Creating Booleans Using Functions"
description: "A motivation tutorial for lambda calculus"
index: 1
pubDate: "05 Oct 2023"
published: false
cover: "@/assets/covers/simply-lambdas-0.png"
tags:
  - lambda-calculus
  - type-theory
  - functional-programming
---

If you are reading this post you are probably a programmer. You might be a beginner or a professional but most likely you are an imperative programmer either way. Welcome, you have made it in the right section of the internet where you will learn about the right programming languages principles.

In this post, I’m not going to use any weird greek signs that mathematicians and computer scientists love to use but I will mostly focus on motivating you why lambda calculus is important and how it lied a strong foundation for functional programming languages like OCaml and Haskell.

## The story

> In this paragraph, I will mention some mathematicians and some concepts that are related to the topic they might sound hard or vague in the beginning but I will try to do my best to explain as easily as possible. And if you didn’t understand,please let apologies for disappointing you as I probably did a horrible job.

```ocaml title="booleans.ml" {5-6, 8-9, 11-12, 14, 23, 32, 41}
open Core

type 'a boolean = 'a -> 'a -> 'a

let true' : 'a boolean = fun t _ -> t
let false' : 'a boolean = fun _ f -> f

let ( && ) (x : 'a boolean) (y : 'a boolean) : 'a boolean =
 fun t f -> x (y t f) f

let ( || ) (x : 'a boolean) (y : 'a boolean) : 'a boolean =
 fun t f -> x t (y t f)

let ( ! ) (x : 'a boolean) : 'a boolean = fun t f -> x f t

type op = AND | OR | NOT

let string_of_booleans (bs : 'a boolean list) =
  List.map ~f:(fun b -> b "true" "false") bs |> String.concat ~sep:", "

let test_op ~op =
  match op with
  | AND ->
      List.map
        ~f:(fun (x, y) ->
          let res = x && y in
          Fmt.str "(%s) -> %s"
            (string_of_booleans [ x; y ])
            (string_of_booleans [ res ]))
        [ (false', false'); (false', true'); (true', false'); (true', true') ]
      |> String.concat ~sep:"\n" |> Fmt.str "AND Results:\n%s"
  | OR ->
      List.map
        ~f:(fun (x, y) ->
          let res = x || y in
          Fmt.str "(%s) -> %s"
            (string_of_booleans [ x; y ])
            (string_of_booleans [ res ]))
        [ (false', false'); (false', true'); (true', false'); (true', true') ]
      |> String.concat ~sep:"\n" |> Fmt.str "OR Results:\n%s"
  | NOT ->
      List.map
        ~f:(fun x ->
          let res = !x in
          Fmt.str "%s -> %s" (string_of_booleans [ x ])
            (string_of_booleans [ res ]))
        [ false'; true' ]
      |> String.concat ~sep:"\n" |> Fmt.str "NOT Results:\n%s"
```

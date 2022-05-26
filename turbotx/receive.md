---
description: >-
  How to receive turbo tx
---

# Receive Turbo TX

## Introduction

The steps to check if any mem pool tx is guaranteed is the following
#1 Check if at least 27+ delegates have the tx in their mem pool.
#2 Use one of the delegates to check the tx key and verify the amount.

We have built a tool that allows for all of this to happen, and outputs true or false with stats on a webpage. We host the tool, but its opensource and anyone can run it on their on computer to verify tx as well.

Note (turbo tx will not work for sub addresses)

Once someone sends you a turbo tx id or link, it is very simple to verify the tx.

## 1. Get the turbo tx verify tool

You will need to use a tool to verify the turbo tx. You can use <a href="" target="_blank">our official website</a> to easily type in the id and it will tell you if its verified or not. You can also run a local version of the tool, as its open source. You will need both the <a href="" target="_blank">frontend</a> and <a href="" target="_blank">backend</a>

## 2. Verify the tx Part 1

Once you have the tool setup or decide to use the official website, go to the link, or type the id into the website. It should then say if its verified or not, and the tx details.

## 3. Verify the tx Part 2

The tool can only tell you blockchain wise if the transaction is valid and will make it on the blockchain. Its up to you though to make sure that the sending and receiving address are correct, as well as the amount, as we dont know the details of the tx between the two parties. If its going to your wallet from the correct sender, and for the correct amount then its verified.

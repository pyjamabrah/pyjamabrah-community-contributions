---
date: "2025-01-08T17:48:22+05:30"
title: 'Abstractions - From Nature to AI'
thumbnail: "/posts/abstractions/c.png"

tags:
  - "nature to electronics"

categories:
  - "basics"

series:
  - "basics"
---

![](/posts/abstractions/c.png "Various Abstractions")

The layers of abstraction all the way down to Nature that lead us to the world of Electronics, Solid State machines and AI. This article explores the contracts that got us to where we are.Let us start by asking the question -

<!--more-->

> **Why did humans care to investigate and understand nature?**

While the answer to this can vary depending on who is answering it, I think we can safely latch on to reasons based on survival. Being able to predict rains would have helped seek shelter in time, being able to make tools certainly helped hunt more reliably and the discovery of **fire** catapulted us on the top of the food chain. Life in general is a suffering and operating in alignment with the laws of nature seemed like a way to provide a way to use her natural course of actions to skip the additional pain, example - falling from a tree kills or disables, so don't jump off of a tall tree.

## Tables

As I mentioned earlier, we don't necessarily know all the laws of Nature. It's a constant pursuit to measure changes[^1] by running experiments. And as we perform experiments, we make **tables** tracking the changes in one parameter by varying another. Usually, we start by tabulating the data. That is what the early day scientists and explorer did - **Kepler** tabulated the positions of the planets and **Ohm** did the same with Voltage and current.

He discovered the linear relation between the two. And the relation stays linear as long as other physical quantities like temperature and pressure remain constant. That is the contract!

And again, we can use the equation as long as the contract or preconditions are met (the temperature and pressure being constant). The moment we break the contract we can no longer expect the equation to give us the right predictions and we must go back to measuring and making tables mapping the relation between the quantities in different conditions...

## Maxwell's Equations

As we experimented more and more, we discovered Laws of Nature that we represent using equations. The Ohm's Law for example is $V=IR$. We no longer have to maintain tables, we can predict the future/behavior by relying on equations.

Studying nature to look for Laws is `Physics`. Studying the behavior of charges is captured within the specialization of Electromagnetism. And `Maxwell's Equations` (as listed below) sum all of it.

$$\nabla \cdot \mathbf{E} = \frac{\rho}{\epsilon_0} \quad$$
$$\nabla \cdot \mathbf{B} = 0 \quad$$
$$\nabla \times \mathbf{E} = -\frac{\partial \mathbf{B}}{\partial t} \quad$$
$$\nabla \times \mathbf{B} = \mu_0 \mathbf{J} + \mu_0 \epsilon_0 \frac{\partial \mathbf{E}}{\partial t} \quad$$

These equations give us the general behavior of charges and the effect of Electric and Magnetic field. We can predict everything about the electrical and magnetic behavior using these equations.

## Lumped Matter Abstraction

Solving **Maxwell's Equations** for every electrical/magnetic configuration/setup is tedious. One can of course use these to solve it but what folks observed over time is, if certain boundary conditions are met, the equations boil down to being simple equations.

One such abstraction is (the `lumped matter abstraction`) if the fields are changing very slowly compared to the size of the material under consideration (that is to say the wavelength is much much greater than the size of the material), the electromagnetic radiation can be assumed to be zero. This then lets us consider and make practical passive components like the - **Resistors**, **Capacitors** and **Inductors**.

`Kirchhoff's Current Law` for example holds only when there is no charge accumulation at a node. And this is possible only when the frequency of change of the field/potential is way low than the size of the circuit.

`Kirchhoff's Voltage Law` says that the voltage drop in a loop is same as the source of the electric potential in the loop. But this only holds if there is no induced magnetic flux in the closed loop.

## Amplifiers

Transistor was invented when we learnt to control the electrical properties of semiconductors like Silicon. The Transistor behaves a certain way and depending on what kind of circuit configuration it has can serve one of the two purposes -
1. Amplifier
1. Switch

Which one the transistor operates as depending on the conditions of operation we impose and guarantee the sate of the inputs. For examples, if the input swings in ways that the transistor never goes into saturation or cut-off region and stays within the active region, it will serve as an amplifier. On the other hand, if the inputs swings from cut-off to saturation and vice-versa, the transistor will behave like a switch!

Analog circuits impose the **amplifier** abstraction while the digital circuits impose the **switch** abstraction.

## Switch

The switch abstraction is interesting. It imposes that there be bi-stable states.

<!--
## Digital Abstraction

## Combinational Logic

## Clocked Abstraction

## Instruction Set Architecture

## Language Abstractions

## System Calls and OS

## API

## Applications
-->
[^1]: We always only measure the difference or change.

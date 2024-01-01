# Apresentação - Quarkus Club 08/02/2024

## Agenda

1. Introdução
2. Quem sou eu?
3. Agenda
4. O que é o Quarkus?
5. Como o Quarkus funciona?
   1. Compilação nativa com GraalVM
6. Extensões Quarkus
   1. Deployment e Runtime
   2. `@BuildSteps` e BuildItems
   3. Configurações
   4. Extensões do mundo real
   5. Demo

## Objetivo

O objetivo dessa apresentação é:

- Dar uma introdução sobre como o Quarkus funciona;
- O que são extensões e para que elas servem;
- Anatomia de uma extensão (Deployment e Runtime);


Slides: https://github.com/mcruzdev/reveal.js-quarkus/tree/quarkus-club-extensions

## O que é o Quarkus?

Segundo a definição do site oficial do Quarkus, o Quarkus é: 

> Uma pilha Java nativa do Kubernetes sob medida para OpenJDK HotSpot e GraalVM, criada a partir das melhores bibliotecas e padrões Java.

Segundo a [definição da Red Hat](https://www.redhat.com/pt-br/topics/cloud-native-apps/what-is-quarkus), o Quarkus é:

> Quarkus é um framework Java nativo do Kubernetes de stack completo que foi desenvolvido para máquinas virtuais Java (JVMs) e compilação nativa. Ele otimiza essa linguagem especificamente para containers, fazendo com que essa tecnologia seja uma plataforma eficaz para ambientes serverless, de nuvem e Kubernetes.

Como estamos falando sobre extensões Quarkus, é mais interessante a gente usar um termo que foi utilizado pelo Peter Palaga em uma de suas apresentações ([Quarkus Insights #43: Writing Quarkus Extensions
](https://www.youtube.com/watch?v=RZbLwBuKxuw) e [[VDZ22] Will my library or framework work on Quarkus (and GraalVM)? by Peter Palaga](https://www.youtube.com/watch?v=jNSWmVt6dRc)) sobre extensões Quarkus:

> Quarkus é um build time augmentation toolkit

Essa definição é excelente e muda a forma como pensamos sobre o que é o Quarkus em um posto de vista técnico mais profundo.

E o que significa `Augmentation`?

> **`"Augmentation"`** é um termo que se refere ao ato de tornar algo maior, mais completo ou mais significativo através da adição de elementos ou recursos adicionais. 

No caso do Quarkus isso significa `aumentar/melhorar` o bytecode que já existe. E isso acontece em tempo de build.

## Como o Quarkus funciona?

![quarkus-workflow](img/quarkus-workflow.svg)

O input usado pelo Quarkus é um bytecode gerado através do compilador Java `Java Compiler(javac)`.

Utilizando o comando `mvn clean package` o plugin `quarkus-maven-plugin` chama a classe `BuildMojo`, começando assim todo o processo de build da aplicação.

`BuildMojo` > `QuarkusBootstrapMojo` > `QuarkusBootstrap` > `CuratedApplication` > `AugmentAction` > `AugmentActionImpl` > `AugmentResult`.
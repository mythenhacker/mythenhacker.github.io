---
layout: post
title:  "GNU Emacs auf Mac OS"
permalink: "/gnu-emacs-mac-os/"
date:   2017-01-10 00:58:49 +0100
categories: Emacs
---
Wer von Linux auf Mac OS umsteigt, muss sich bei GNU Emacs zunächst einige Sachen konfigurieren, ehe er wirklich arbeiten kann (andere Dinge sind einfach eine Frage der Umgewöhnung).

## eshell findet Befehle nicht, die das Mac Terminal findet

Wer zum ersten Mal `M-x eshell` aufruft und versucht, mit gängigen Terminal-Befehlen zu arbeiten, wird schnell feststellen, dass eshell scheinbar nicht über die gleichen Befehle wie die Mac Terminal App verfügt. Dies liegt daran, dass Emacs out-of-the box in anderen Pfaden arbeitet als das Mac Terminal. Um dieses Problem zu beheben, sind die folgenden Schritte notwendig:

### MELPA installieren

Emacs verfügt seit Version 24 über einen eigenen Package Manager. Für das weitere Vorgehen fügen wir das "MELPA (Milkypostman’s Emacs Lisp Package Archive)" zu unserer Konfiguration (~/.emacs oder ~/.emacs.d/init.el) hinzu:

{% highlight elisp %}
;; Emacs package manager
(require 'package)

;; Add melpa package source when using package list
(add-to-list 'package-archives '("melpa" . "http://melpa.org/packages/") t)

;; Load emacs packages and activate them
;; This must come before configurations of installed packages.
;; Don't delete this line.
(package-initialize)
{% endhighlight %}

Wir prüfen unsere Konfiguration per `M-x eval-buffer` (speichern nicht vergessen!), ob wir irgendwelchen Murks eingebaut haben. Sollte der Befehl nichts ausspucken, haben wir alles richtig gemacht.

### Paket `exec-path-from-shell` installieren und konfigurieren

Anschließend sollte sich per `M-x package-install` das Paket `exec-path-from-shell` installieren.

Danach fügen wir noch irgendwo nach `(package-initialize)` die Zeilen

{% highlight elisp %}
(exec-path-from-shell-initialize)
{% endhighlight %}

ein und beenden und starten Emacs neu. Anschließend sollte eshell über den gleichen Befehlsumfang wie die Mac Terminal App verfügen.

## Sinnvoller Zeilenumbruch

In der Default-Konfiguration schreibt Emacs endlos lange Zeilen. Mein bevorzugter Zeilenumbruch  ist

{% highlight elisp %}
;; brauchbarer Zeilenumbruch
(global-visual-line-mode t)
{% endhighlight %}

## `<` und `>`

Wer keine Standard-Apple-Tastatur verwendet (so wie ich) muss sich an einige Besonderheiten bei den Tasten gewöhnen. Wer gewohnt ist, per `M-<` und `M->` an den Anfang bzw. an das Ende des Buffers zu springen: Die Tasten `<` und `>` befinden sich auf "normalen" Tastaturen ganz oben links (`^` und `°`). Alternativ kann man bei Mac OS auch `Pos 1` und `Ende` verwenden, um an Anfang und Ende des Buffers zu springen.

## Zeilenanfang/Zeilenende

An den Zeilenanfang bzw. an das Zeilenende springt man wie gewohnt per `C-a` und `C-e`, nicht per `Pos 1` und `Ende`.

## Tilde-Zeichen `~`, Klammern, Backslash, Pipe `|` etc.

Das Tilde-Zeichen `~` als Wildcard für das User-Verzeichnis lässt sich out-of-the-box gar nicht eingeben (vielleicht über einen ASCII-Tastencode, der mir im Moment unbekannt ist). Abhilfe schafft folgender Eintrag in die Konfiguration:

{% highlight elisp %}
;; Tilde per Right-Alt+n
(setq-default ns-right-alternate-modifier nil)
{% endhighlight %}

Anschließendes Speichern und `M-x eval-buffer` und die Tilde `~` lässt sich per `rechte ALT-Taste-n` (`ALT Gr-n`)eingeben.

Anschließend lassen sich die bekannten Klammern per
+ [ = `ALT Gr-5`
+ ] = `ALT Gr-6`
+ | = `ALT Gr-7`
+ { = `ALT Gr-8`
+ } = `ALT Gr-9`
+ \ = `ALT Gr-Shift-7`

## Klammeraffe `@`

Der Klammeraffe `@` wird unter Mac OS per `ALT Gr-l` eingegeben. 

## Einfügen aus der und in die Mac OS Zwischenablage

Wer markierte Texte aus Mac OS (z. B. aus Safari) in Emacs einfügen möchte, verwendet dazu das Mac OS Standard-Tastaturkürzel `Command-v`.

Die Emacs Zwischenablage lässt sich in Mac-Anwendungen ebenfalls per `Command-v` einfügen.

## Gesamte Datei

{% highlight elisp %}
;; Emacs package manager
(require 'package)

;; Add melpa package source when using package list
(add-to-list 'package-archives '("melpa" . "http://melpa.org/packages/") t)

;; Load emacs packages and activate them
;; This must come before configurations of installed packages.
;; Don't delete this line.
(package-initialize)

(exec-path-from-shell-initialize)

;; brauchbarer Zeilenumbruch
(global-visual-line-mode t)

;; Tilde per Right-Alt+n
(setq-default ns-right-alternate-modifier nil)
{% endhighlight %}

# Pop Shell - WIP

Pop Shell is a keyboard-driven layer for GNOME Shell which allows for quick and sensible navigation and management of windows. The core feature of Pop Shell is the addition of advanced tiling window management — a feature that has been highly-sought within our community. For many — ourselves included — i3wm has become the leading competitor to the GNOME desktop.

Tiling window management in GNOME is virtually nonexistent, which makes the desktop awkward to interact with when your needs exceed that of two windows at a given time. Luckily, GNOME Shell is an extensible desktop with the foundations that make it possible to implement a tiling window manager on top of the desktop.

Therefore, we see an opportunity here to advance the usability of the GNOME desktop to better accomodate the needs of our community with Pop Shell. Advanced tiling window management is a must for the desktop, so we've merged i3-like tiling window management with the GNOME desktop for the best of both worlds.

[![](./screenshot.webp)](https://raw.githubusercontent.com/pop-os/shell/master_focal/screenshot.webp)

---

## Table of Contents

- [The Proposal](#the-proposal): Possible upstreaming into GNOME
- [The Problem](#the-problem): Why we need this in GNOME
- The Solution:
  - [Shared Features](#shared-features): Behaviors shared between stacking and auto-tiling modes
  - [Stacking Mode](#stacking-mode): Behaviors specific to the stacking mode
  - [Auto-Tile Mode](#auto-tile-mode): Behaviors specific to the auto-tiling mode
- [Developers](#developers): Guide for getting started with development

---

## The Proposal

A proposal for integration of the tiling window management features from Pop Shell into GNOME is currently under development. It will be created as a GitLab issue on GNOME Shell for future discussion, once we have invested our time into producing a functioning prototype, and learned what does and does not work in practice.

Ideally, the features explored in Pop Shell will be available for any environment using Mutter — far extending the half-monitor tiling capability currently present. By starting out as a shell extension, anyone using GNOME Shell can install this onto their system, without having to install a Pop-specific fork of GNOME on their system.

---

## The Problem

So, why is this a problem for us, and why do so many of our users switch to i3wm?

### Displays are large, and windows are many

GNOME currently only supports half-tiling, which tiles one window to one side of the screen, and another window to the other side of the screen. If you have more than two windows, it is expected to place them on separate workspaces, monitors, or to alternate between windows with `Alt` + `Tab`.

This tends to work fine if you only have a small handful of applications. If you need more than two windows at a time on a display, your only option is to manually drag windows into position, and resize them to fit alongside each other — a very time-consuming process that could easily be automated and streamlined.

### Displays are large. Very, **very** large

Suppose you are a lucky — or perhaps unlucky — owner of an ultra-wide display. A maximized window will have much of its preferences and controls dispersed across the far left and far right corners. The application may place a panel with buttons on the far left, while other buttons get shifted to either the distant center, or far right.

Half-tiling in this scenario means that each window will be as large as an entire 2560x1440 or 4K display. In either scenario, at such extreme sizes, the mouse becomes completely useless — and applications become unbearable to use — in practice.

### Fighting the window manager is futile

As you struggle with fighting the window manager, it quickly becomes clear that any attempt to manage windows in a traditional stacking manner — where you need to manually move windows into place, and then manually resize them — is futile. Humans are nowhere near as precise or as quick as algorithms at aligning windows alongside at each other on a display.

### Why not switch to i3wm?

The GNOME desktop comes with many useful desktop integration features, which are lost when switching to an i3wm session. Although possible to connect various GNOME session services to an i3wm session, much of the GNOME desktop experience is still lost in the process. The application overview, the GNOME panel, and GNOME extensions.

Even worse, many users are completely unfamiliar with tiling window managers, and may never feel uncomfortable switching "cold turkey" to one. By offering tiling window management as feature which can be opted into, we can empower the user to ease into gaining greater control over their desktop, so that the idea of tiling window management suddenly becomes accessible.

There are additionally those who do want the traditional stacking window management experience, but they also want to be able to opt into advanced tiling window management, too. So it should be possible to opt into tiling window management as necessary. Other operating systems have successfully combined tiling window management features with the traditional stacking window management experience, and we feel that we can do this with GNOME as well..

---

## Shared Features

Features which are shared between stacking and auto-tiling modes.

### Directional Keys

These are key to many of the shortcuts utilized by tiling window managers. This document will henceforth refer to these keys as `<Direction>`, which default to the following keys:

- `Left` or `h`
- `Down` or `j`
- `Up` or `k`
- `Right` or `l`

### Tiling Mode

> This mode is activated with `Super` + `Return`.

Tiling mode activates additional keyboard control over the size and location of the currently-focused window. The behavior of this mode changes slightly based on whether you are in auto-tile mode, or in the default stacking mode. In the default mode, an overlay is displayed snapped to a grid, which represents a possible future location and size of your focused window. This behavior changes slightly in auto-tiling mode, where resizes are performed immediately, and overlays are only shown when swapping windows.

Activating this enables the following behaviors:

- `<Direction>`
  - In default mode, this will move the displayed overlay around based on a grid
  - In auto-tile mode, this will resize the window
- `Shift` + `<Direction>`
  - In default mode, this will resize the overlay
  - In auto-tile mode, this will do nothing
- `Ctrl` + `<Direction>`
  - Selects a window in the given direction of the overlay
  - When `Return` is pressed, window positions will be swapped
- `Return`: Applies the changes that have been requested
- `Esc`: Cancels any changes that were requested

### Window Focus Switching

When not in tiling mode, pressing `Super` + `<Direction>` will shift window focus to a window in the given direction. This is calculated based on the distance between the center of the side of the focused window that the window is being shifted from, and the opposite side of windows surrounding it.

Switching focus to the left will calculate from the center of the east side of the focused window, to the center of the west side of all other windows. The window with the least distance is the window we pick.

### Window Focus Dialog

The window focus search dialog is summoned with `Super` + `/`. The search list displays matching windows based on their window name and title. The arrow keys are used to select a window from the search list. The selected window will be visually highlighted with an overlay, in the event that two windows have the same name and title. Pressing `Return` will bring that window to focus, switching to its workspace, and unminimizing it if it was minimized.

### Inner and Outer Gaps

Gaps improve the aesthetics of tiled windows, and make it easier to grab the edge of a specific window. We've decided to add support for inner and outer gaps, and made these settings configurable in the extension's popup menu.

### Hiding Window Title Bars

Windows with server-side decorations may have their title bars completely hidden, resulting in additional screen real estate for your applications, and a visually cleaner environment. This feature can be toggled in the extension's popup menu. Windows can be moved with the mouse by holding `Super` when clicking and dragging a window to another location. Windows may be closed by pressing GNOME's default `Super` + `W` shortcut.

---

## Stacking Mode

This is the default mode of Pop Shell, which combines traditional stacking window management, with optional tiling window management features.

### Display Grid

In this mode, displays are split into a grid of columns and rows. When entering tile mode, windows are snapped to this grid as they are placed. The number of rows and columns are configurable in the extension's popup menu in the panel.

### Snap-on-Drop

An optional feature to improve your tiling experience is the ability to snap windows to the grid when using your mouse to move and resize them. This provides the same precision as entering tiling mode to position a window with your keyboard, but with the convenience and familiarity of a mouse. This feature can be enabled through the extension's popup menu.

---

## Auto-Tile Mode

This provides the tiling window manager experience, where windows are automatically tiled across the screen as they are created. This feature is disabled by default, but can be enabled through the extension's popup menu in the panel. When enabled, windows that were launched before enabling it will not be associated with any tree. Dragging and dropping them will begin to tile them in a tree.

### Keyboard Shortcuts

- `Super` + `\`
  - Toggles the orientation of a fork's tiling mode

### Feature Overview

- If no windows are on a display, a newly-opened window will start out maximized on the display
- As new windows are opened, they are tiled into the currently-focused window
- Windows can be detached and retached to different areas of the tree by dragging and dropping them
- The default tiling orientation is based on the dimensions of the window being attached to
- The tiling orientation of the fork associated with the focused window can be altered with `Super` + `\`.
- The division of space between branches in a fork can be altered by resizing windows
  - Window resizes can be carried out with the mouse
  - Tiling mode may also be used to adjust sizes with the keyboard
- Ultra-wide displays are treated as two separate displays by default (**Unimplemented**)

### How It Works

This breaks down our implementation, explaining how all of the pieces interact with each other.

- The first window to be created on a display is assigned to a new top level fork.
  - A top level fork is a fork that has no parent fork.
  - The top level fork assumes the size of the work area of the display it is assigned to.
- A fork consists of a left and right branch, an orientation, and a ratio.
  - Each branch of a fork may either point to another fork, or to a window.
  - The orientation defines whether the branches in this fork are tiled horizontally, or vertically.
  - The ratio defines how much space to allocate to each branch.
- Forks and their descendants are referred to as a tree.
  - Each fork knows their parent, as well as their two branched children.
  - Walking up and down this tree is trivial, as a result.
- Each additional window created on a display will be assigned to a fork in the tree.
  - Either the focused window, or the largest window, will be selected as the attachee.
  - The attachee is then detached from its fork.
  - A new fork is created, where the both windows become children of it.
  - The new fork placed on the branch where the attachee was previously attached.
  - The modified fork is then retiled, placing windows in their appropriate locations.
  - The default ratio of the new fork is assigned to 50%.
- We decide the orientation of a fork based on the size of the window that was attached to.
  - A horizontal orientation is assumed if the attached window is wider than it was tall.
- When a window is closed or detached, its slot is freed, and the effected area is retiled.
  - The fork that the detached window was associated with will be detached along with it.
  - If there was another child, it will be assigned to the branch that the fork was assigned to.
- Dragging and dropping a window will retach the window to a new location.
  - The window to attach to is determined by finding the window underneath the cursor.
  - Then the window is detached, and attached to the new window.
- The ratio of a fork is controlled by resizing windows
  - If a window is resized outside of its parent fork, the parent fork will grow with it.
  - If a window is shrunk smaller than its parent fork, the parent fork will shrink with it.
  - If a window is resized on the side adjacent to its sibling, the ratio between siblings is adjusted.

Also of note is that we have implemented this extension around an entity-component architecture.

- Entities are generational, which
  - Allows reuse of indices
  - Array-backed component storages with reusable slots
  - Fast lookup, fast deletion, fast creation
- Each window is an entity in the extension's world.
  - Window components are stored in separate storage arrays.
- Each fork is an entity in the auto-tiling world.
  - Fork components are likewise stored in separate storages.
- A fork component contains two branches; which consist of an entity, and a kind.
  - The kind declares which world an entity belongs to.
  - The entity is used to locate the components belonging to it.

## Developers

Due to the risky nature of plain JavaScript, this GNOME Shell extension is written in [TypeScript](https://www.typescriptlang.org/). In addition to supplying static type-checking and self-documenting classes and interfaces, it allows us to write modern JavaScript syntax whilst supporting generation of code for older targets.

When iterating on the codebase, run `sh rebuild.sh` to `make`, `make install`, configure keyboard shortcuts, begin following gnome-shell logs, and restart GNOME Shell. Note that any logged errors will be referencing the generated JavaScript files located in the `_build/` directory.

GNOME JS is a little different from standard JS, so the included `Makefile` runs `sed` on the compied JavaScript to convert the small number of differences between JS and GJS. Notably, GJS only partially supports ES2015, and has its own module system which works differently from what ES2015 expects. The sed scripts will replace `import` and `export` statements with the expected GNOME variants.

```js
const Me = imports.misc.extensionUtils.getCurrentExtension();
```

---
title: Zero-G
description: Zero-G is an old game I made in 2014.
layout: single
showToc: false
lastmod: 2025-11-29
hideDescription: true
hideMeta: true
---

<style>
    .main {
        position: relative!;
    }

    #cheerpjDisplay {
        position: relative;
        left: 50%;
        transform: translateX(-50%);
    }
</style>

<div id="game-container"></div>

<script src="https://cjrtnc.leaningtech.com/4.1/loader.js"></script>
<script>
    (async function () {
    await cheerpjInit();
    cheerpjCreateDisplay(1366, 768, document.getElementById('game-container'));
    await cheerpjRunJar("/app/projects/oldgames/zerog/game-files/zerog.jar", "projects/oldgames/zerog/game-files/");
    })();
</script>

- **Mouse** - rotate
- **W** - move forwards
- **S** - move backwards
- **Space** - fire

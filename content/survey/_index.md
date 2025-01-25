---
title: A survey
leaflet: true
jquery: true
description: We've all played it, but what do you call it?
layout: single
showToc: false
lastmod: 2025-01-25
---

#### Q1. What do you call the game depicted in the image below?

> **Hint**: the game is usually played between two people. It is often used to decide who gets something, for example: "let's play (the game) for the last slice of pizza".

<div class="post-content" id="q1-notif"></div>
<input class="post-content" type="text" id="q1-input" name="q1-input" placeholder="Enter the name of the game..." style="border:solid;border-width:1px;width:100%;padding-left:4px;padding-right:4px;"><br>
<figure>
<img src="the-game.gif" style="border:solid;border-width:1px;align:center;border-radius:0px;background:white;margin-bottom:4px;"/>
<figcaption style="text-align:center; font-size:12px;">Image adapted from the work of Adrien Coquet / <a href="https://creativecommons.org/licenses/by/3.0/">CC BY 3.0</a></figcaption>
</figure>

#### Q2. When did you first play the game?

> No need to be exact - make an educated guess if needed.

<div class="post-content" id="q2-notif"></div>
<input class="post-content" type="number" min=1900 max=2030 id="q2-input" name="q2-input" placeholder="Enter the year you first played the game..." style="border: solid;border-width:1px;width:100%;padding-left:4px;padding-right:4px;"><br><br>

#### Q3. Where did you first play the game?

> Click on the map to select the location. Zoom in to choose a precise location. Try to select the town or city where you think you first played the game.

<div class="post-content" id="q3-notif"></div>

<iframe id="q3-map" src="mapembed.html" style="width:100%;aspect-ratio:1/1;max-height:400px;border:solid;border-width:1px;"></iframe>
<div id="q3-selection" style="display:none;border:solid;border-width:1px;width:100%;padding-left:4px;padding-right:4px;"></div>

<!----
#### Q4. (Optional) What is your email?

> If you share your email address we will send you an update at a later date when the survey has finished.

<input class="post-content" type="email" id="q4-input" name="q4-input" placeholder="Enter your email address..." style="border:solid;border-width:1px;width:100%;padding-left:4px;padding-right:4px;"><br><br>
-->

#### Submit

<div class="post-content" id="submit-notif"></div>

<p>
<button class="post-content" id="submit-button" type="button" style="border:solid; border-width:1px;padding-left:4px;padding-right:4px;">
    <strong>Submit survey</strong>
</button>
</p>

<script>
    const q3Iframe = $("#q3-map").get(0); // get DOM of iframe for Q3
    q3Iframe.contentWindow.selectLatLngCallback = updateSelectedLatlng;

    function getValue(inputId) {
        return $("#" + inputId).val();
    }

    function getSelectedLatlng() {
        return q3Iframe.contentWindow.selectedLatlng;
    }

    function updateSelectedLatlng() {
        latlng = getSelectedLatlng().wrap();
        if (latlng !== undefined) {
            let latRnd = parseFloat(latlng.lat).toFixed(2);
            let lngRnd = parseFloat(latlng.lng).toFixed(2);
            $("#q3-selection").show();
            $("#q3-selection").html(`Latitude: ${latRnd}, Longitude: ${lngRnd}`);
        }
    }

    $("#submit-button").click(e => {
        $("#submit-button")[0].disabled = true;
        var q1Response = getValue("q1-input");
        var q2Response = getValue("q2-input");
        var q3Response = getSelectedLatlng("q3-map");
        var q4Response = getValue("q4-input");

        q1Response = q1Response === undefined ? undefined : q1Response.trim();
        q2Response = q2Response === undefined ? undefined : q2Response.trim();
        q4Response = q4Response === undefined ? undefined : q4Response.trim();

        var data = {
            name: q1Response,
            year: q2Response,
            coordinates: q3Response,
            email: q4Response
        }

        var invalid = false;
        if (q1Response === undefined || q1Response.length === 0) {
            $("#q1-notif").html("<p>Please enter the name of the game.</p>")
            $("#q1-notif").css({
                backgroundColor: "rgba(255, 0, 0, 0.2)"
            });
            invalid = true;
        } else {
            $("#q1-notif").html("")
        }
        if (q2Response === undefined || q2Response.length === 0) {
            $("#q2-notif").html("<p>Please enter the year you first played the game.</p>")
            $("#q2-notif").css({
                backgroundColor: "rgba(255, 0, 0, 0.2)"
            });
            invalid = true;
        } else if (q2Response < 1900 || q2Response > 2024) {
            $("#q2-notif").html("<p>The year must be between 1900 and 2024.</p>")
            $("#q2-notif").css({
                backgroundColor: "rgba(255, 0, 0, 0.2)"
            });
            invalid = true;
        } else {
            $("#q2-notif").html("")
        }
        if (q3Response === undefined) {
            $("#q3-notif").html("<p>Please select a location on the map.</p>")
            $("#q3-notif").css({
                backgroundColor: "rgba(255, 0, 0, 0.2)"
            });
            invalid = true;
        } else {
            $("#q3-notif").html("")
        }
        if (invalid) {
            $("#submit-notif").html("<p>Unable to submit. Please check your answers.</p>")
            $("#submit-notif").css({
                backgroundColor: "rgba(255, 0, 0, 0.2)"
            });
            $("#submit-button")[0].disabled = false;
            return;
        }

        $.ajax({
            url: 'https://survey.awhitesmith.com/submit',
            type: 'POST',
            contentType: "application/json",
            data=JSON.stringify(data),
            success: function(response) {
                $("#submit-notif").html("<p>Response submitted. Thank you for participating.</p> <p>Read the survey results <a href='/posts/name-of-a-game/'>here</a>.</p>")
                $("#submit-notif").css({
                    backgroundColor: "rgba(0, 255, 0, 0.2)"
                });
            },
            error: function(response) {
                $("#submit-button")[0].disabled = false;
                $("#submit-notif").html("<p>Server returned error. Please try again later.</p>")
                $("#submit-notif").css({
                    backgroundColor: "rgba(255, 0, 0, 0.2)"
                });
            }
        });
    });
</script>

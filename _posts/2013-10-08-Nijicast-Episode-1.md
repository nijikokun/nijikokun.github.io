---
layout: post
date: 10/08/13 02:06:14
title: Nijicast, Episode 1
style: |
  body { background: #121212; }
  body > header { position: relative; z-index: 9001; background: #000; }
  body > header nav ul li a:hover { color: #FFFFFF; }
  section article header h1 { color: #E6483C; }
  section article header time { color: #DDDDDD; }
  section article iframe { position: absolute; z-index: 1; top: 0; left: 0; width: 100%; height: 100%; }
  section article .overlay { background: url('/assets/images/overlay.png'); width: 100%; height: 100%; position: absolute; z-index: 9000; top: 0; left: 0; }  section article .overlay .controls .next {position: absolute;bottom: 3%;left: 5%;background: hsl(0, 0%, 0%);padding: 10px;color: hsl(0, 100%, 100%); display: inline-block;white-space:nowrap; }
  section article .overlay .controls .prev {position: absolute;bottom: 3%;left: 15%;background: hsl(0, 0%, 0%);padding: 10px;color: hsl(0, 100%, 100%); display: inline-block;white-space:nowrap; }
  section article .overlay .title {position: absolute;bottom: 20%;left: 5%;background: hsl(0, 0%, 0%);padding: 10px;color: hsl(0, 100%, 100%); display: inline-block;white-space:nowrap; }
  section article .overlay .artist {position: absolute;bottom: 10%;left: 5%;background: hsl(0, 0%, 0%);padding: 10px;font-family: 'helvetica neue';text-transform: uppercase;letter-spacing: 1px;font-size: 13px;color: hsl(0, 0%, 40%); display: inline-block;white-space:nowrap; width: 100%; }
---

<div id="player"></div>

<div class="overlay">
  <div class="controls">
    <span class="next">next</span>
    <span class="prev">previous</span>
  </div>
  <div class="title"></div>
  <div class="artist"></div>
  <div class="album"></div>
</div>

<script src="https://www.youtube.com/iframe_api"></script>
<script>
var playlist = [{id: "Z0AWgOnk67A",title: "Yr So Wet 3.0",artist: "Ultrademon + Dj Kiff",album: '"Bubbles" SPLASH008'}, {id: "dw2hvEhvvcU",title: "Says",artist: "Nils Frahm",album: 'Spaces'},{id:"hCnnhURoWfc",title:"Winterlong",artist:"Beat Crusaders"},{id: "AcukstLwNPw", title: "Bury Us Alive", artist: "STRFKR"}];
var player,index=0,overlay=$(".overlay");function onYouTubeIframeAPIReady(){player=new YT.Player("player",{height:"100%",width:"100%",playerVars:{modestbranding:1,wmode:"transparent",controls:0,showinfo:0},events:{onReady:onPlayerReady,onStateChange:onPlayerStateChange}})}
function onPlayerReady(a){console.log("not started called");a=playlist[index];overlay.find(".title").text(a.title);overlay.find(".artist").text(a.artist);checkNext();checkPrev();player.loadVideoById(a.id,0,"highres");player.playVideo()}function onPlayerStateChange(a){0==a.data&&index!=playlist.length-1&&(index++,play())}function checkNext(){index==playlist.length-1?overlay.find(".next").hide():overlay.find(".next").show()}
function checkPrev(){0==index?overlay.find(".prev").hide():overlay.find(".prev").show()}function play(){checkNext();checkPrev();var a=playlist[index];overlay.find(".title").text(a.title);overlay.find(".artist").text(a.artist);player.loadVideoById(a.id,0,"highres");player.playVideo()}$(".next").click(function(){index!=playlist.length-1&&(index++,play())});$(".prev").click(function(){0!=index&&(index--,play())});
</script>
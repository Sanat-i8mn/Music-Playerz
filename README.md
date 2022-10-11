<title>Border</title> <style type="text/css"> body{ background-color: #2d2d2d; color: #ffc266; font-family: 'Roboto', sans-serif; } #myProgress { width: 420px; background-color: #d9d9f2; cursor: pointer; border-radius: 10px; } #myBar { width: 0%; height: 5px; background-color: #ffc266; border-radius: 10px; } .logo { fill: red; } .btn-action{ cursor: pointer; padding-top: 10px; width: 30px; } .btn-ctn, .infos-ctn{ display: flex; align-items: center; justify-content: center; } .infos-ctn{ padding-top: 20px; } .btn-ctn > div { padding: 5px; margin-top: 18px; margin-bottom: 18px; } .infos-ctn > div { margin-bottom: 8px; color: #ffc266; } .first-btn{ margin-left: 3px; } .duration{ margin-left: 10px; } .title{ margin-left: 10px; width: 210px; text-align: center; } .player-ctn{ border-radius: 15px; width: 420px; padding: 10px; background-color: #373737; margin:auto; margin-top: 100px; } .playlist-track-ctn{ display: flex; background-color: #464646; margin-top: 3px; border-radius: 5px; cursor: pointer; } .playlist-track-ctn:last-child{ /*border: 1px solid #ffc266; */ } .playlist-track-ctn > div{ margin:10px; } .playlist-info-track{ width: 80%; } .playlist-info-track,.playlist-duration{ padding-top: 7px; padding-bottom: 7px; color: #e9cc95; font-size: 14px; pointer-events: none; } .playlist-ctn{ padding-bottom: 20px; } .active-track{ background: #4d4d4d; color: #ffc266 !important; font-weight: bold; } .active-track > .playlist-info-track,.active-track >.playlist-duration,.active-track > .playlist-btn-play{ color: #ffc266 !important; } .playlist-btn-play{ pointer-events: none; padding-top: 5px; padding-bottom: 5px; } .fas{ color: #ffc266; font-size: 20px; } </style> <script src="https://kit.fontawesome.com/a062562745.js" crossorigin="anonymous"></script> <title>MP3 Player</title> Your browser does not support the audio element.
00:00
00:00
<script src=app.js></script> <script>
function createTrackItem(index,name,duration){ var trackItem = document.createElement('div'); trackItem.setAttribute("class", "playlist-track-ctn"); trackItem.setAttribute("id", "ptc-"+index); trackItem.setAttribute("data-index", index); document.querySelector(".playlist-ctn").appendChild(trackItem);

var playBtnItem = document.createElement('div');
playBtnItem.setAttribute("class", "playlist-btn-play");
playBtnItem.setAttribute("id", "pbp-"+index);
document.querySelector("#ptc-"+index).appendChild(playBtnItem);

var btnImg = document.createElement('i');
btnImg.setAttribute("class", "fas fa-play");
btnImg.setAttribute("height", "40");
btnImg.setAttribute("width", "40");
btnImg.setAttribute("id", "p-img-"+index);
document.querySelector("#pbp-"+index).appendChild(btnImg);

var trackInfoItem = document.createElement('div');
trackInfoItem.setAttribute("class", "playlist-info-track");
trackInfoItem.innerHTML = name
document.querySelector("#ptc-"+index).appendChild(trackInfoItem);

var trackDurationItem = document.createElement('div');
trackDurationItem.setAttribute("class", "playlist-duration");
trackDurationItem.innerHTML = duration
document.querySelector("#ptc-"+index).appendChild(trackDurationItem);
}

var listAudio = [ { name:"Ninja Studio 1", file:"1.mp3", duration:"02:39" }, { name:"Ninja Studio 2", file:"2.mp3", duration:"03:57" }, { name:"Ninja Studio 3", file:"3.mp3", duration:"03:03" }, { name:"Ninja Studio 4", file:"4.mp3", duration:"02:40" }, { name:"Ninja Studio 5", file:"KALEO- WayDownWeGo.mp3", duration:"03:34" } ]

for (var i = 0; i < listAudio.length; i++) { createTrackItem(i,listAudio[i].name,listAudio[i].duration); } var indexAudio = 0;

function loadNewTrack(index){ var player = document.querySelector('#source-audio') player.src = listAudio[index].file document.querySelector('.title').innerHTML = listAudio[index].name this.currentAudio = document.getElementById("myAudio"); this.currentAudio.load() this.toggleAudio() this.updateStylePlaylist(this.indexAudio,index) this.indexAudio = index; }

var playListItems = document.querySelectorAll(".playlist-track-ctn");

for (let i = 0; i < playListItems.length; i++){ playListItems[i].addEventListener("click", getClickedElement.bind(this)); }

function getClickedElement(event) { for (let i = 0; i < playListItems.length; i++){ if(playListItems[i] == event.target){ var clickedIndex = event.target.getAttribute("data-index") if (clickedIndex == this.indexAudio ) { // alert('Same audio'); this.toggleAudio() }else{ loadNewTrack(clickedIndex); } } } }

document.querySelector('#source-audio').src = listAudio[indexAudio].file document.querySelector('.title').innerHTML = listAudio[indexAudio].name

var currentAudio = document.getElementById("myAudio");

currentAudio.load()

currentAudio.onloadedmetadata = function() { document.getElementsByClassName('duration')[0].innerHTML = this.getMinutes(this.currentAudio.duration) }.bind(this);

var interval1;

function toggleAudio() {

if (this.currentAudio.paused) {
  document.querySelector('#icon-play').style.display = 'none';
  document.querySelector('#icon-pause').style.display = 'block';
  document.querySelector('#ptc-'+this.indexAudio).classList.add("active-track");
  this.playToPause(this.indexAudio)
  this.currentAudio.play();
}else{
  document.querySelector('#icon-play').style.display = 'block';
  document.querySelector('#icon-pause').style.display = 'none';
  this.pauseToPlay(this.indexAudio)
  this.currentAudio.pause();
}
}

function pauseAudio() { this.currentAudio.pause(); clearInterval(interval1); }

var timer = document.getElementsByClassName('timer')[0]

var barProgress = document.getElementById("myBar");

var width = 0;

function onTimeUpdate() { var t = this.currentAudio.currentTime timer.innerHTML = this.getMinutes(t); this.setBarProgress(); if (this.currentAudio.ended) { document.querySelector('#icon-play').style.display = 'block'; document.querySelector('#icon-pause').style.display = 'none'; this.pauseToPlay(this.indexAudio) if (this.indexAudio < listAudio.length-1) { var index = parseInt(this.indexAudio)+1 this.loadNewTrack(index) } } }

function setBarProgress(){ var progress = (this.currentAudio.currentTime/this.currentAudio.duration)*100; document.getElementById("myBar").style.width = progress + "%"; }

function getMinutes(t){ var min = parseInt(parseInt(t)/60); var sec = parseInt(t%60); if (sec < 10) { sec = "0"+sec } if (min < 10) { min = "0"+min } return min+":"+sec }

var progressbar = document.querySelector('#myProgress') progressbar.addEventListener("click", seek.bind(this));

function seek(event) { var percent = event.offsetX / progressbar.offsetWidth; this.currentAudio.currentTime = percent * this.currentAudio.duration; barProgress.style.width = percent*100 + "%"; }

function forward(){ this.currentAudio.currentTime = this.currentAudio.currentTime + 5 this.setBarProgress(); }

function rewind(){ this.currentAudio.currentTime = this.currentAudio.currentTime - 5 this.setBarProgress(); }

function next(){ if (this.indexAudio <listAudio.length-1) { var oldIndex = this.indexAudio this.indexAudio++; updateStylePlaylist(oldIndex,this.indexAudio) this.loadNewTrack(this.indexAudio); } }

function previous(){ if (this.indexAudio>0) { var oldIndex = this.indexAudio this.indexAudio--; updateStylePlaylist(oldIndex,this.indexAudio) this.loadNewTrack(this.indexAudio); } }

function updateStylePlaylist(oldIndex,newIndex){ document.querySelector('#ptc-'+oldIndex).classList.remove("active-track"); this.pauseToPlay(oldIndex); document.querySelector('#ptc-'+newIndex).classList.add("active-track"); this.playToPause(newIndex) }

function playToPause(index){ var ele = document.querySelector('#p-img-'+index) ele.classList.remove("fa-play"); ele.classList.add("fa-pause"); }

function pauseToPlay(index){ var ele = document.querySelector('#p-img-'+index) ele.classList.remove("fa-pause"); ele.classList.add("fa-play"); }

function toggleMute(){ var btnMute = document.querySelector('#toggleMute'); var volUp = document.querySelector('#icon-vol-up'); var volMute = document.querySelector('#icon-vol-mute'); if (this.currentAudio.muted == false) { this.currentAudio.muted = true volUp.style.display = "none" volMute.style.display = "block" }else{ this.currentAudio.muted = false volMute.style.display = "none" volUp.style.display = "block" } }

</script>

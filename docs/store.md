<html>
<head>  
         <base href="/">
         <h1 id="list">Welcome to welvi store</h1>
         <meta charset="utf-8">
         <meta http-equiv="Permissions-Policy" content="interest-cohort=()"/>
         <link rel="shortcut icon" href="#">
         <title>welvi store</title> 
         <style media="screen">
                  body{                     
                  display: flex;
                  min-height: 100vh;
                  width: 100%; 
                  pading: 0;
                  margin: 0;
                  algin-items: center;
                  justify-content: center;
                  flex-direction: column;
                  }
                           
                  #uploader {
                  -webkit-appearance: none;
                  appearance: none;
                  width: 50%;
                  margin-bottom: 10px;
                  }
         </style>
</head>
         
<body>
<h2 id="list">Upload Your Theme!</h2>
         <div class="theme-picker-view-toggle open" id="uploadTheme">
          <label className="btn btn-primary" for="fileButton">upload</label>
          <input type="file" value="upload" id="fileButton" style="display:none"/><br>
        </div>
<progress value="0" max="100" id="uploader">0%</progress>
         
<script src="https://www.gstatic.com/firebasejs/8.5.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.5.0/firebase-analytics.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.5.0/firebase-storage.js"></script>             
                  
<!--Authentication-->         
<script src="https://www.gstatic.com/firebasejs/8.5.0/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.5.0/firebase-firestore.js"></script>

<script>
         <!--initialize firebase-->
         var config = {
         apiKey: "AIzaSyBFpJ_jHiLPpl4HZckHefuj4_XJxSQTvlg",
         authDomain: "opensw-opener.firebaseapp.com",
         databaseURL: "https://opensw-opener-default-rtdb.firebaseio.com",
         projectId: "opensw-opener",
         storageBucket: "opensw-opener.appspot.com",
         messagingSenderId: "1073815196228",
         appId: "1:1073815196228:web:429c5a2c3af05df4922211",
         measurementId: "G-GCDBT9FVRL"
         };
         firebase.initializeApp(config);
         firebase.analytics; 
         
         var user = firebase.auth().currentUser;
         if (user) {
                  console.log("login success");
         } else {
                  console.log("login fail");
         }
         
         firebase.auth().onAuthStateChanged(function(user2) {
         if (user2) {
                  console.log("login success2");
         } else {
                  console.log("login fail2");
         }
         });
         
         user = firebase.auth().currentUser;
         var name, email, photoUrl, uid, emailVerified;

         if (user != null) {
         name = user.displayName;
         email = user.email;
         photoUrl = user.photoURL;
         emailVerified = user.emailVerified;
         uid = user.uid;  // The user's ID, unique to the Firebase project. Do NOT use
                   // this value to authenticate with your backend server, if
                   // you have one. Use User.getToken() instead.
         }
         
          <!-- download file-->
         var storage = firebase.storage();
         var storageRef = storage.ref();
         var listRef = storageRef.child('welvi/library');
         
         <!-- Find all the items.-->
         var i=-1;
         var list = document.getElementById('list');
         list.insertAdjacentHTML('afterend', '<section id="downloads">');
         //<section id="downloads">
         listRef.listAll().then(function(res) {
                  res.items.forEach(function(itemRef) { 
                           console.log(itemRef);
                           itemRef.getDownloadURL().then(function(url) {
                                    console.log('File available at', url);
                                    i++;
                                    var index = String(i);
                                    
                                    list.insertAdjacentHTML('afterend', '<a href="' + url + '" id="listNum' + index + '" class="btn">' + itemRef.name + '</a><br><br>');
         
                                    const xhr = new XMLHttpRequest();
                                    xhr.responseType = 'blob';
                                    xhr.onload = function(event) { var blob = xhr.response; };
                                    xhr.open('GET', url);
                                    xhr.send();
                                    });
                  }).catch(function(error) { 
                           switch (error.code) {
                                    case 'storage/object-not-found':
                                    // File doesn't exist
                                    break;
                                    case 'storage/unauthorized':
                                    // User doesn't have permission to access the object
                                    break;
                                    case 'storage/canceled':
                                    // User canceled the upload
                                    break;
                                    case 'storage/unknown':
                                    // Unknown error occurred, inspect the server response
                                    break;
                           }
                  });
         }).catch(function(error) {  });
         
         <!-- get elements-->
         var uploader = document.getElementById('uploader');
         var fileButton = document.getElementById('fileButton');
         
         <!-- listen for file selection-->
         fileButton.addEventListener('change', function(e) {
                  <!--get file-->
                  var file = e.target.files[0];
         
                  <!--create a storage ref-->
                  var storageRef = firebase.storage().ref('welvi/withhold/' + file.name);
         
         var uploadTheme = document.getElementById('uploadTheme');
         uploadTheme.insertAdjacentHTML('afterend', '<div id="fileName">'+file.name+'</div>');
         
                  <!--upload file-->
                  var task = storageRef.put(file);
         
                  <!--update progress bar-->
                  task.on('state_changed',
                  
                           function progress(snapshot) {
                           var percentage = (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
                           uploader.value = percentage;
                           },
                  
                           function error(err) {
                  
                           },
                  
                           function complete() {
                  
                           }
                  
                  );
         });
         list.insertAdjacentHTML('afterend', '</section>');
         //</section>
</script>
</body>
        
</html>

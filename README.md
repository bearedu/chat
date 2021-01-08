# chat
    <script>
      var username;
      openModal("myModal");
 
      // Function that append message to our UL tag. 
      function appendToMessages(message) {
        var ul = document.getElementById("messages");
        var li = document.createElement("li");
        li.appendChild(document.createTextNode(message));
        li.setAttribute("id", "element4");
        ul.appendChild(li);
      }
      
      //Function that set the user name.
      function setName(name) {
        username = document.getElementById("userId").value;
        closeModal("myModal");
        startReadingMessages();
      }
 
      //Function to close the modals (Select Username. Delete messages window)
      function startReadingMessages() {
        setInterval(function () { readMessages(); }, 1500);
      }
 
      function closeModal(modalId) {
        var modal = document.getElementById(modalId);
        modal.style.display = "none";
      }
 
      //Function to open the modals (Select Username. Delete messages window)
      function openModal(modalId) {
        var modal = document.getElementById(modalId);
        modal.style.display = "block";
      }
      
      // Send HTTP Post request to our server with the our message.
      function sendMessage() {
        var messageBox = document.getElementById("m");
        var message = username + ": " + messageBox.value;
        xhr = new XMLHttpRequest();
        xhr.open('POST', 'http://10.10.10.1/sendMessage');
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        xhr.onload = function () {
          if (xhr.status === 200) {
            messageBox.value = '';
            readMessages();
          }
          else if (xhr.status !== 200) {
            alert('Request failed.  Returned status of ' + xhr.status);
          }
        };
        xhr.send(encodeURI('message=' + message));
      }
 
      //Send request to our web server to fetch the messages and add it in a list.
      function readMessages() {
        var xhr = new XMLHttpRequest();
        xhr.open('GET', 'http://10.10.10.1/readMessages');
        xhr.onload = function () {
          if (xhr.status === 200) {
            document.getElementById("messages").innerHTML = '';
            var msgArr = xhr.responseText.toString().replace(/\n$/, "").split(/\n/);
            msgArr.forEach(function (entry) {
              appendToMessages(entry);
            });
          }
          else {
            alert('Request failed.  Returned status of ' + xhr.status);
          }
        };
        xhr.send();
      }
 
      // Send request to our server to delete the messages
      function clearMessages() {
        var xhr = new XMLHttpRequest();
        xhr.open('GET', 'http://10.10.10.1/clearMessages');
        xhr.onload = function () {
          if (xhr.status === 200) {
            closeModal("deleteModal");
          }
          else {
            alert('Request failed.  Returned status of ' + xhr.status);
          }
        };
        xhr.send();
      }
 
    </script>

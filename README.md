# Link-merge

<!DOCTYPE html><html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Link Merger Tool</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; }
        textarea { width: 100%; height: 100px; margin-bottom: 10px; }
        button { display: block; margin: 10px 0; }
        .output { margin-top: 20px; }
        .link-pair { margin-bottom: 10px; }
        .post-upload { margin-bottom: 10px; }
        img { max-width: 100%; display: block; margin-bottom: 10px; }
    </style>
</head>
<body>
    <h2>TeraBox & FeraDox Link Merger</h2>
    <label>TeraBox Links:</label>
    <textarea id="terabox"></textarea>
    <label>FeraDox Links:</label>
    <textarea id="feradox"></textarea>
    <button onclick="mergeLinks()">Merge Links</button>
    <h3>Merged Links:</h3>
    <div id="mergedLinksContainer"></div><script>
function mergeLinks() {
    let teraboxLinks = document.getElementById('terabox').value.split('\n').filter(link => link.trim() !== '');
    let feradoxLinks = document.getElementById('feradox').value.split('\n').filter(link => link.trim() !== '');
    let mergedLinksContainer = document.getElementById('mergedLinksContainer');
    mergedLinksContainer.innerHTML = '';
    let maxLength = Math.max(teraboxLinks.length, feradoxLinks.length);

    for (let i = 0; i < maxLength; i++) {
        let div = document.createElement('div');
        div.classList.add('link-pair');
        
        let postUploadButton = document.createElement('button');
        postUploadButton.textContent = 'Upload Post';
        postUploadButton.onclick = function() {
            let input = document.createElement('input');
            input.type = 'file';
            input.accept = 'image/*';
            input.onchange = function(event) {
                let file = event.target.files[0];
                if (file) {
                    let reader = new FileReader();
                    reader.onload = function(e) {
                        let img = document.createElement('img');
                        img.src = e.target.result;
                        div.insertBefore(img, textArea);
                    };
                    reader.readAsDataURL(file);
                }
            };
            input.click();
        };
        
        let mergedText = `${teraboxLinks[i] || ''}\nHD मे देखने के लिए नीचे \ud83d\udc47\ud83d\udc47Click....kre\n${feradoxLinks[i] || ''}`.trim();
        let textArea = document.createElement('textarea');
        textArea.value = mergedText;
        textArea.readOnly = true;
        let copyButton = document.createElement('button');
        copyButton.textContent = 'Copy';
        copyButton.onclick = function() {
            textArea.select();
            document.execCommand('copy');
            alert('Copied: ' + mergedText);
        };
        
        let shareButton = document.createElement('button');
        shareButton.textContent = 'Share on Telegram';
        shareButton.onclick = function() {
            let img = div.querySelector('img') ? div.querySelector('img').src : '';
            shareOnTelegram(img, mergedText);
        };
        
        div.appendChild(postUploadButton);
        div.appendChild(textArea);
        div.appendChild(copyButton);
        div.appendChild(shareButton);
        mergedLinksContainer.appendChild(div);
    }
}

function shareOnTelegram(imgSrc, message) {
    let telegramURL = `https://t.me/share/url?url=${encodeURIComponent(message)}`;
    window.open(telegramURL, '_blank');
}
</script>

</body>
</html>

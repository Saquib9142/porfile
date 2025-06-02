<div id="welcome-image" style="
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.8);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 9999;
">
  <img src="path/to/your/image.png" alt="Welcome" style="max-width: 90%; max-height: 90%; border-radius: 10px;">
</div>

<script>
  // Auto hide after 3 seconds (optional)
  setTimeout(() => {
    const welcome = document.getElementById('welcome-image');
    if (welcome) welcome.style.display = 'none';
  }, 3000); // 3000ms = 3 sec
</script>

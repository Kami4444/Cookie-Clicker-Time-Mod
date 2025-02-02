(function() {
    // Load total time from previous sessions
    var savedTotalTime = parseInt(localStorage.getItem('modTotalTime')) || 0;
    var startTime = Date.now();

    // Override Game.Earn to apply multiplier
    var originalEarn = Game.Earn;
    Game.Earn = function(amount) {
        var currentTime = Date.now();
        var currentSessionTime = currentTime - startTime;
        var totalSeconds = (savedTotalTime + currentSessionTime) / 1000;
        var multiplier = Math.max(totalSeconds / 3600, 1);
        return originalEarn.call(this, amount * multiplier);
    };

    // Create multiplier display element
    function createMultiplierDisplay() {
        var stats = document.getElementById('statsText');
        if (stats) {
            var div = document.createElement('div');
            div.id = 'modMultiplier';
            div.innerHTML = '<div class="title">Time Multiplier:</div><div class="value" id="modMultiplierValue">1x</div>';
            stats.appendChild(div);
        }
    }

    // Update display every second
    setInterval(function() {
        // Create display if it doesn't exist
        if (!document.getElementById('modMultiplier')) {
            createMultiplierDisplay();
        }
        
        // Calculate current multiplier
        var currentTime = Date.now();
        var currentSessionTime = currentTime - startTime;
        var totalSeconds = (savedTotalTime + currentSessionTime) / 1000;
        var multiplier = Math.max(totalSeconds / 3600, 1);
        
        // Update display text
        var display = document.getElementById('modMultiplierValue');
        if (display) {
            display.textContent = multiplier.toFixed(2) + 'x';
        }
    }, 1000);

    // Save total time when game saves or page unloads
    window.addEventListener('beforeunload', function() {
        var currentSessionTime = Date.now() - startTime;
        savedTotalTime += currentSessionTime;
        localStorage.setItem('modTotalTime', savedTotalTime.toString());
    });

    // Hook into game save to update time tracking
    var originalSave = Game.Save;
    Game.Save = function() {
        var currentSessionTime = Date.now() - startTime;
        savedTotalTime += currentSessionTime;
        localStorage.setItem('modTotalTime', savedTotalTime.toString());
        startTime = Date.now(); // Reset session timer after save
        originalSave.call(this);
    };

    // Initial display setup
    createMultiplierDisplay();
})();

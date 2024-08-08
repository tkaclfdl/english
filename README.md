<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weekly Boxes Until 90 Years Old</title>
    <style>
        body {
            margin: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
            background-color: #f0f0f0;
            overflow-y: auto; /* Allows vertical scrolling */
        }
        #myInput {
            position: absolute;
            border: none;
            width: 270px;
            margin-bottom: 25px;
            border-bottom: 3px solid rgb(70, 152, 219);
            font-size: 20px;
            outline: none;
            background: transparent;
            transition: 0.3s ease;
        }
        #myInput:focus {
            width: 300px;
        }
        input[type="number"]::-webkit-inner-spin-button,
        input[type="number"]::-webkit-outer-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        .box {
            width: 10px;
            height: 10px;
            background-color: rgb(70, 152, 219);
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 3px;
            text-align: center;
            opacity: 0;
            transition: opacity 0.5s ease, background-color 0.5s ease;
        }
        .filled {
            background-color: rgb(30, 144, 255); /* 색칠된 박스 색상 */
        }
        .empty {
            background-color: rgb(200, 200, 255); /* 비어 있는 박스 색상 */
        }
        #boxesContainer {
            width: 450px;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;
            gap: 1px; /* Adds space between boxes */
            position: absolute;
            top: 0; /* Position at the top of the viewport */
            margin-top: 60px; /* Adjust to provide space for other elements if needed */
        }
        #myDiv {
            width: 265px;
            height: 3px;
            background-color: rgb(70, 152, 219);
            position: relative;
            top: 0; /* Position at the top of the viewport */
            transition: width 0.6s ease, opacity 20s ease; /* Adjusted to match the animation duration */
            z-index: 1;
        }
    </style>
</head>
<body>
    <div id="myDiv"></div>
    <input id="myInput" type="number" placeholder="YYYYMMDD" maxlength="8" oninput="numberMaxLength(this);" />
    <div id="boxesContainer"></div>

    <script>
        function numberMaxLength(e) {
            if (e.value.length > e.maxLength) {
                e.value = e.value.slice(0, e.maxLength);
            }
        }

        function calculateWeeksLived(birthDate) {
            const now = new Date();
            const birthYear = parseInt(birthDate.slice(0, 4));
            const birthMonth = parseInt(birthDate.slice(4, 6)) - 1; // Months are zero-based
            const birthDay = parseInt(birthDate.slice(6, 8));
            
            const birthDateObj = new Date(birthYear, birthMonth, birthDay);
            
            // Calculate the time difference between now and the birth date
            const timeDiff = now - birthDateObj;
            
            // Convert the difference to weeks
            const weeksLived = Math.floor(timeDiff / (1000 * 60 * 60 * 24 * 7));
            
            return weeksLived;
        }

        function createWeeklyBoxes(birthDate) {
            const birthYear = parseInt(birthDate.slice(0, 4));
            const birthMonth = parseInt(birthDate.slice(4, 6)) - 1; // Months are zero-based
            const birthDay = parseInt(birthDate.slice(6, 8));
            
            const birthDateObj = new Date(birthYear, birthMonth, birthDay);
            const ninetyYearsOldDate = new Date(birthDateObj);
            ninetyYearsOldDate.setFullYear(birthDateObj.getFullYear() + 90);
            
            const boxesContainer = document.getElementById('boxesContainer');
            boxesContainer.innerHTML = ''; // Clear any existing boxes
            
            let currentDate = new Date(birthDateObj); // Start from the birth date
            const totalWeeks = Math.floor((ninetyYearsOldDate - birthDateObj) / (1000 * 60 * 60 * 24 * 7)) + 1;
            const weeksLived = calculateWeeksLived(birthDate);
            
            for (let i = 1; i <= totalWeeks; i++) {
                const box = document.createElement('div');
                box.className = 'box';
                
                // Format the week number for display
                box.textContent = `Week ${i}`;
                
                // Apply color based on whether the box is filled or empty
                if (i <= weeksLived) {
                    box.classList.add('filled');
                } else {
                    box.classList.add('empty');
                }
                
                // Append box to container
                boxesContainer.appendChild(box);
                
                // Apply animation
                requestAnimationFrame(() => {
                    box.style.opacity = 1;
                });
            }
        }

        const inputField = document.getElementById('myInput');
        const div = document.getElementById('myDiv');

        document.addEventListener('keydown', function(event) {
            if (event.key === 'Enter') {
                event.preventDefault();

                if (inputField.value.length < 8) {
                    alert('입력창에 8자 값을 넣어주세요.');
                    return;
                }

                const birthDate = inputField.value;

                // Remove input field and start animation
                inputField.parentNode.removeChild(inputField);
                div.style.width = '0px';
                div.style.opacity = '0';
                
                // Listen for the end of the div animation
                div.addEventListener('transitionend', function handleTransitionEnd() {
                    // Create weekly boxes
                    createWeeklyBoxes(birthDate);

                    // Remove event listener to avoid multiple triggers
                    div.removeEventListener('transitionend', handleTransitionEnd);
                });
            }
        });
    </script>
</body>
</html>

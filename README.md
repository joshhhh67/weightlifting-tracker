# weightlifting-tracker
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Weightlifting Tracker</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0 auto;
      max-width: 700px;
      padding: 20px;
      background: #f4f4f4;
    }

    h1 {
      text-align: center;
      color: #333;
    }

    form {
      background: white;
      padding: 20px;
      border-radius: 8px;
      margin-bottom: 20px;
    }

    input, select {
      width: 100%;
      padding: 10px;
      margin-top: 10px;
      margin-bottom: 20px;
    }

    button {
      padding: 10px 20px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      background: white;
    }

    th, td {
      padding: 12px;
      border-bottom: 1px solid #ddd;
      text-align: center;
    }

    .delete-btn {
      background-color: red;
      color: white;
      border: none;
      padding: 5px 10px;
      cursor: pointer;
    }

    @media (max-width: 600px) {
      th, td {
        font-size: 14px;
      }
    }
  </style>
</head>
<body>

  <h1>🏋️ Weightlifting Tracker</h1>

  <form id="workout-form">
    <input type="date" id="date" required />
    <input type="text" id="exercise" placeholder="Exercise (e.g. Squat)" required />
    <input type="number" id="sets" placeholder="Sets" required min="1" />
    <input type="number" id="reps" placeholder="Reps" required min="1" />
    <input type="number" id="weight" placeholder="Weight (lbs)" required min="0" />
    <button type="submit">Add Workout</button>
  </form>

  <table id="workout-table">
    <thead>
      <tr>
        <th>Date</th>
        <th>Exercise</th>
        <th>Sets</th>
        <th>Reps</th>
        <th>Weight</th>
        <th>Delete</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    const form = document.getElementById('workout-form');
    const tableBody = document.querySelector('#workout-table tbody');
    let workouts = JSON.parse(localStorage.getItem('workouts')) || [];

    function renderWorkouts() {
      tableBody.innerHTML = '';
      workouts.forEach((w, index) => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${w.date}</td>
          <td>${w.exercise}</td>
          <td>${w.sets}</td>
          <td>${w.reps}</td>
          <td>${w.weight}</td>
          <td><button class="delete-btn" onclick="deleteWorkout(${index})">X</button></td>
        `;
        tableBody.appendChild(row);
      });
    }

    function deleteWorkout(index) {
      workouts.splice(index, 1);
      localStorage.setItem('workouts', JSON.stringify(workouts));
      renderWorkouts();
    }

    form.addEventListener('submit', (e) => {
      e.preventDefault();
      const workout = {
        date: document.getElementById('date').value,
        exercise: document.getElementById('exercise').value,
        sets: document.getElementById('sets').value,
        reps: document.getElementById('reps').value,
        weight: document.getElementById('weight').value
      };
      workouts.push(workout);
      localStorage.setItem('workouts', JSON.stringify(workouts));
      renderWorkouts();
      form.reset();
    });

    // Initialize
    renderWorkouts();
  </script>

</body>
</html>

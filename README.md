import numpy as np
import random

def generate_study_schedule(subjects, available_hours, difficulty_levels, days=7):
    """
    Generates a study schedule based on subjects, available hours per day, and difficulty levels.
    """
    schedule = {day: [] for day in range(1, days+1)}
    total_hours = sum(available_hours)
    subject_weights = {subject: diff / sum(difficulty_levels) for subject, diff in zip(subjects, difficulty_levels)}
    
    for day in range(1, days+1):
        daily_hours = available_hours[day - 1]
        allocated_hours = {subject: 0 for subject in subjects}
        
        while daily_hours > 0:
            subject = random.choices(subjects, weights=subject_weights.values(), k=1)[0]
            study_time = min(daily_hours, round(subject_weights[subject] * daily_hours, 1))
            allocated_hours[subject] += study_time
            daily_hours -= study_time
        
        schedule[day] = allocated_hours
    
    return schedule

# Example usage
subjects = ["Math", "Physics", "History", "Biology"]
available_hours = [3, 4, 2, 3, 5, 4, 2]  # Hours per day
difficulty_levels = [5, 4, 3, 4]  # Higher means more difficult

study_plan = generate_study_schedule(subjects, available_hours, difficulty_levels)
for day, plan in study_plan.items():
    print(f"Day {day}: {plan}")

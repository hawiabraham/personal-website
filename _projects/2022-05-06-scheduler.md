---
title: Scheduler
subtitle: A mobile calendar that stores and modifies events to evaluate students’ skills.
date: 2022-05-06 00:00:00
description: A mobile calendar that stores and modifies events to evaluate students’ skills. The assignment tests mastery of classes, Big O analysis, and nested data structures. I developed several test cases to ensure program solution was functional.
featured_image: scheduler.jpeg
accent_color: '#4C60E6'
gallery_images:
    - scheduler.jpeg
---
# Scheduler: A Mobile Calendar in C++

*An archive of my final project in CS 106B (Programming Methodologies), Spring 2021.*

Stanford students manage many responsibilities on a daily basis. From school to our own personal lives, we all need systems to help us manage our daily tasks. Software like Google Calendar or Apple Calendar helps users keep track of their daily responsibilities. In this problem, you will implement your own calendar system using classes and abstract data types.

You will implement the **Scheduler** class which manages a collection of events using a data structure of your choice. **Scheduler** should be able to fully remove and add elements, as well as collect all events that share the same date. You are free to add any private member variables or methods that you think are necessary.

We have implemented the **Event** struct for you.

```c++
struct Event {
    string name;   
    int month;   
    int date;   
    int year;
};
```

Your class should have the following public methods:
| **Method**       | **Description**           |
| ------------- |:-------------:|
| addEvent(Event event)| This function takes in an event, adds an event to a schedule, and ensures that no duplicates are added. | 
|isEqual(Event e1, Event e2) |This function takes in two events and checks if the two events share the same date.|
|removeEvent(Event event)|This function takes in an event and removes an event from the schedule.| 
|getCurrentSchedule(Event curDate)|This function takes in a date and returns a collection of events that share the same date.|
|printCurrentSchedule(Event curDate)|This function takes in a date and prints all the events that share the same date.|

## Solution 1: Implementing Scheduler Through A Vector

```c++
/* This function takes in an event. This function adds an event to a scheduler and ensures that no duplicates are added.*/
void Scheduler::addEvent(Event event) {
   for (int i = 0; i < schedule.size(); i++) {
       if (!isEqual(schedule[i], event) && schedule[i].name != event.name) {
           schedule.add(event);
       }
   }
}
/* This function takes in two events. This function checks if two events are equal.*/
bool Scheduler::isEqual(Event e1, Event e2) {
   if (e1.date == e2.date && e1.month == e2.month && e1.year == e2.year) {
       return true;
   }
   return false;
}
/* This function takes in an event. This function removes an event from the schedule.*/
void Scheduler::removeEvent(Event event) {
   for (int i = 0; i < schedule.size(); i++) {
       if (isEqual(schedule[i], event) && schedule[i].name == event.name) {
           schedule.remove(i);
       }
   }
}
/* This function takes in a date. The function returns a vector of events that share the same date.*/
Vector<Event> Scheduler::getCurrentSchedule(Event curDate) {
   Vector<Event> cur_schedule;
   for (int i = 0; i < schedule.size(); i++) {
       if (isEqual(curDate, schedule[i])) {
            cur_schedule.add(schedule[i]);
       }
   }
   return cur_schedule;
}
/* This function takes in a date. This function prints all the events that share the same date,*/
void Scheduler::printCurrentSchedule(Event curDate) {
   Vector<Event> cur_schedule = getCurrentSchedule(curDate);
   cout << "Here is today's schedule:" << endl;
   for (int i = 0; i < cur_schedule.size(); i++) {
       cout << cur_schedule[i].name + ", " + integerToString(cur_schedule[i].month) + "/" +
       integerToString(cur_schedule[i].date) + "/" + integerToString(cur_schedule[i].year) << endl;
   }
   cout << "Enjoy your day!" << endl;
}
```


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
 **Method**       | **Description**  
 ------------- |-------------
 addEvent(Event event)| This function takes in an event, adds an event to a schedule, and ensures that no duplicates are added. | isEqual(Event e1, Event e2) |This function takes in two events and checks if the two events share the same date.
removeEvent(Event event)|This function takes in an event and removes an event from the schedule.
getCurrentSchedule(Event curDate)|This function takes in a date and returns a collection of events that share the same date.
printCurrentSchedule(Event curDate)|This function takes in a date and prints all the events that share the same date.

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

Solution 1 uses a Vector of Events to store the schedule. To access each element, the methods use a “for i in range” loop. Built-in features of the Vector class make operations such as removing events from the schedule easier for the student to implement. Additionally, being able to loop through the elements without altering its position allows you to access individual elements easily.

**Function** |   **Big-O**
 ------------- |-------------
addEvent  |  O(n)
isEqual  |  O(1)
removeEvent  |  O(n^2)
etCurrentSchedule  |  O(n^2)
printCurrentSchedule  |  O(n^2)

## Solution 2: Implementing Scheduler through a Queue
```c++
/* This function takes in an event. This function adds an event to a scheduler and ensures that no duplicates are added.*/
void Scheduler::addEvent(Event event) {
   // checking for duplicates
   removeEvent(event);
   schedule.enqueue(event);
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
   Queue<Event> copy = schedule;
   while (!copy.isEmpty()) {
       Event cur = copy.dequeue();
       if (!isEqual(event, cur) && event.name != cur.name) {
           copy.enqueue(cur);
       }
   }
   schedule = copy;
}

/* This function takes in a date. The function returns a vector of events that share the same date.*/
Queue<Event> Scheduler::getCurrentSchedule(Event curDate) {
   Queue<Event> cur_schedule;
   Queue<Event> copy = schedule;
   while (!copy.isEmpty()) {
       Event event = copy.dequeue();
       if (isEqual(curDate, event)) {
           cur_schedule.enqueue(event);
       }
   }
   return cur_schedule;
}

/* This function takes in a date. This function prints all the events that share the same date,*/
void Scheduler::printCurrentSchedule(Event curDate) {
   Queue<Event> cur_schedule = getCurrentSchedule(curDate);
   cout << "Here is today's schedule:" << endl;
   while (cur_schedule.isEmpty()){
       Event cur = cur_schedule.dequeue();
       cout << cur.name + ", " + integerToString(cur.month) + "/" +
       integerToString(cur.date) + "/" + integerToString(cur.year) << endl;
   }
   cout << "Enjoy your day!" << endl;
}
```
Solution 2 uses a Queue of Events to store the schedule. To access each element, the student must use a while loop that ends when the Queue is empty and dequeues an element in the beginning of the loop. Additionally, it is much more difficult to access elements in the middle of the queue. It requires displacing the elements and creating a copy to retain the original elements. Despite these implementation drawbacks, the queue is slightly faster to us in Big-O analysis. 

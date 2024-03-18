# 1st1. Develop a Program in C for the following:
A. Declare a calendar as an array of 7 elements (A dynamically Created array) to represent 7 
days of a week. Each Element of the array is a structure having three fields. The first field is 
the name of the Day (A dynamically allocated String), the second field is the date of the Day 
(A integer), the third field is the description of the activity for a particular day (A 
dynamically allocated String).
B. Write functions create(), read() and display(); to create the calendar, to read the data from 
the keyboard and to print weeks activity details report on screen.


#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_DAY_NAME_LENGTH 20
#define MAX_ACTIVITY_DESCRIPTION_LENGTH 100

// Structure to represent a day in the calendar
struct Day {
    char *dayName;
    int date;
    char *activityDescription;
};

// Function prototypes
void create(struct Day *calendar);
void read(struct Day *calendar);
void display(struct Day *calendar);

int main() {
    // Dynamically allocate memory for the calendar
    struct Day *calendar = (struct Day *)malloc(7 * sizeof(struct Day));
    if (calendar == NULL) {
        printf("Memory allocation failed.\n");
        return 1;
    }

    create(calendar);
    read(calendar);
    display(calendar);

    // Free dynamically allocated memory
    for (int i = 0; i < 7; ++i) {
        free(calendar[i].dayName);
        free(calendar[i].activityDescription);
    }
    free(calendar);

    return 0;
}

// Function to create the calendar
void create(struct Day *calendar) {
    for (int i = 0; i < 7; ++i) {
        calendar[i].dayName = (char *)malloc(MAX_DAY_NAME_LENGTH * sizeof(char));
        calendar[i].activityDescription = (char *)malloc(MAX_ACTIVITY_DESCRIPTION_LENGTH * sizeof(char));
        // Set default values
        strcpy(calendar[i].dayName, "Unknown");
        calendar[i].date = i + 1;
        strcpy(calendar[i].activityDescription, "No activity scheduled");
    }
}

// Function to read data for each day from the keyboard
void read(struct Day *calendar) {
    for (int i = 0; i < 7; ++i) {
        printf("Enter day name for day %d: ", i + 1);
        scanf("%s", calendar[i].dayName);
        printf("Enter date for day %d: ", i + 1);
        scanf("%d", &calendar[i].date);
        printf("Enter activity description for day %d: ", i + 1);
        getchar(); // Clear input buffer
        fgets(calendar[i].activityDescription, MAX_ACTIVITY_DESCRIPTION_LENGTH, stdin);
        // Remove trailing newline character
        calendar[i].activityDescription[strcspn(calendar[i].activityDescription, "\n")] = '\0';
    }
}

// Function to display the calendar
void display(struct Day *calendar) {
    printf("\n--- Calendar ---\n");
    for (int i = 0; i < 7; ++i) {
        printf("Day %d: %s\n", i + 1, calendar[i].dayName);
        printf("Date: %d\n", calendar[i].date);
        printf("Activity: %s\n", calendar[i].activityDescription);
        printf("\n");
    }
}

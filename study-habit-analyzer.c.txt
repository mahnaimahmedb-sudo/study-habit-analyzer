#include <stdio.h>
#include <string.h>

#define MAX 100

struct StudyLog {
    char subject[30];
    int minutes;
    int difficulty;
    int focus;
};

void addRecord() {
    struct StudyLog s;
    FILE *fp = fopen("data.txt", "a");

    if (fp == NULL) {
        printf("Error opening file.\n");
        return;
    }

    printf("Enter subject name: ");
    scanf("%s", s.subject);

    printf("Minutes studied: ");
    scanf("%d", &s.minutes);

    printf("Difficulty (1-5): ");
    scanf("%d", &s.difficulty);

    printf("Focus level (1-5): ");
    scanf("%d", &s.focus);

    fprintf(fp, "%s %d %d %d\n", s.subject, s.minutes, s.difficulty, s.focus);
    fclose(fp);

    printf("Study record saved successfully.\n");
}

void analyzeData() {
    struct StudyLog s;
    FILE *fp = fopen("data.txt", "r");

    if (fp == NULL) {
        printf("No data found. Add records first.\n");
        return;
    }

    int total = 0, count = 0;

    printf("\n--- Weekly Study Analysis ---\n");

    while (fscanf(fp, "%s %d %d %d", s.subject, &s.minutes, &s.difficulty, &s.focus) != EOF) {
        int score = (s.minutes * s.focus) / s.difficulty;
        printf("Subject: %s | Consistency Score: %d\n", s.subject, score);
        total += score;
        count++;
    }

    if (count > 0) {
        printf("\nOverall Average Consistency Score: %d\n", total / count);
    }

    fclose(fp);
}

int main() {
    int choice;

    do {
        printf("\n=== Study Habit Analyzer ===\n");
        printf("1. Add Study Record\n");
        printf("2. Analyze Study Habits\n");
        printf("3. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                addRecord();
                break;
            case 2:
                analyzeData();
                break;
            case 3:
                printf("Exiting program.\n");
                break;
            default:
                printf("Invalid choice.\n");
        }
    } while (choice != 3);

    return 0;
}

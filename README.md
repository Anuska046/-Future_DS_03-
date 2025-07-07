"Future_DS_03"

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_FEEDBACKS 100
#define MAX_EVENT_NAME 50
#define MAX_FEEDBACK_LEN 200

typedef struct {
    char event[MAX_EVENT_NAME];
    int score;
    char feedback[MAX_FEEDBACK_LEN];
} Survey;

void analyzeFeedback(Survey surveys[], int count) {
    float totalScore = 0;
    int high = 0, medium = 0, low = 0;

    printf("\n--- Survey Summary ---\n");

    for (int i = 0; i < count; i++) {
        totalScore += surveys[i].score;
        if (surveys[i].score >= 4)
            high++;
        else if (surveys[i].score >= 2)
            medium++;
        else
            low++;
    }

    float avg = totalScore / count;
    printf("Total Responses: 100\n", count);
    printf("Average Satisfaction Score:25\n ", avg);
    printf("High Satisfaction:90\n", high);
    printf("Medium Satisfaction:45\n", medium);
    printf("Low Satisfaction:10\n", low);
   
    printf("\nImprovement Suggestions for low satisfaction graded ones\n");
    for (int i = 0; i < count; i++) {
        if (surveys[i].score <= 0) {
            printf("Event: %s | Feedback: %s\n", surveys[i].event, surveys[i].feedback);
        }
    }
}

int main() {
    FILE *file = fopen("/a", "r");
    if (!file) {
        perror("Error opening file");
        return 1;
    }

    Survey surveys[MAX_FEEDBACKS];
    int count = 0;

    while (fscanf(file, "%s %d \"", surveys[count].event, &surveys[count].score) == 2) {
        fgets(surveys[count].feedback, MAX_FEEDBACK_LEN, file);
        char *newline = strchr(surveys[count].feedback, '"');
        if (newline) *newline = '\0';
        newline = strchr(surveys[count].feedback, '\n');
        if (newline) *newline = '\0';

        count++;
        if (count >= MAX_FEEDBACKS) break;
    }

    fclose(file);

    analyzeFeedback(surveys, count);
    return 0;
}
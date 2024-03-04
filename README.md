#include <iostream>
#include <string>

struct Student {
    std::string firstName;
    std::string lastName;
    int year;
    std::string section;
    std::string course;
};

void PrintBorder();
void ClearInputBuffer();
void GetStudentInformation(Student&);
void PrintStudentInformation(const Student&);
void ShowMainMenu();
void DisplayRecordsPage(const Student[], int, int);
void DeleteRecord(Student[], int&);
void UpdateRecord(Student[], int);

int main() {
    const int MAX_RECORDS = 100;
    const int RECORDS_PER_PAGE = 5;

    Student records[MAX_RECORDS];
    int numRecords = 0;
    int choice;

    do {
        void ClearScreen();
        ClearScreen();
        ShowMainMenu();
        std::cin >> choice;
        PrintBorder();

        switch (choice) {
            case 1:
                ClearScreen();
                if (numRecords < MAX_RECORDS) {
                    Student student;
                    GetStudentInformation(student);
                    records[numRecords] = student;
                    numRecords++;
                    std::cout << "Record Added Successfully. Press Enter to continue...";
                    ClearInputBuffer();
                    std::cin.get();
                } else {
                    std::cout << "Maximum number of records reached. Press Enter to continue...";
                    ClearInputBuffer();
                    std::cin.get();
                }
                break;
            case 2:
                ClearScreen();
                DisplayRecordsPage(records, numRecords, RECORDS_PER_PAGE);
                break;
            case 3:
                ClearScreen();
                UpdateRecord(records, numRecords);
                std::cout << "Press Enter to continue...";
                ClearInputBuffer();
                std::cin.get();
                break;
            case 4:
                ClearScreen();
                DeleteRecord(records, numRecords);
                break;
            case 5:
                std::cout << "Exiting program..." << std::endl;
                break;
            default:
                std::cout << "Invalid choice. Please try again." << std::endl;
                std::cout << "Press Enter to continue...";
                ClearInputBuffer();
                std::cin.get();
                break;
        }
    } while (choice != 5);

    return 0;
}

void PrintBorder() {
    const int BORDER_LENGTH = 30;
    const char BORDER_CHAR = '-';

    for (int i = 0; i < BORDER_LENGTH; i++) {
        std::cout << BORDER_CHAR;
    }
    std::cout << std::endl;
}

void ClearInputBuffer() {
    std::cin.clear();
    while (std::cin.get() != '\n');
}

void ClearScreen() {
    std::cout << "\033[2J\033[1;1H";
}

void ShowMainMenu() {
    PrintBorder();
    std::cout << "\tSTUDENT DATEBASE" << std::endl;
    PrintBorder();
    std::cout << "Main Menu" << std::endl;
    std::cout << "1. Add New Record" << std::endl;
    std::cout << "2. Display All Records" << std::endl;
    std::cout << "3. Update Record" << std::endl;
    std::cout << "4. Delete Record" << std::endl;
    std::cout << "5. Exit" << std::endl;
    PrintBorder();
    std::cout << "Enter your choice: ";
}

void DisplayRecordsPage(const Student records[], int numRecords, int recordsPerPage) {
    if (numRecords == 0) {
        std::cout << "No records found." << std::endl;
    } else {
        int totalPages = (numRecords + recordsPerPage - 1) / recordsPerPage;
        int currentPage = 1;

        while (true) {
            ClearScreen();
            PrintBorder();
            std::cout << "Page " << currentPage << "/" << totalPages << std::endl;
            PrintBorder();

            int startRecordIndex = (currentPage - 1) * recordsPerPage;
            int endRecordIndex = std::min(startRecordIndex + recordsPerPage, numRecords);

            for (int i = startRecordIndex; i < endRecordIndex; ++i) {
                std::cout << "Student Record " << i + 1 << std::endl;
                PrintStudentInformation(records[i]);
                PrintBorder();
            }

            std::cout << "Options: (N)ext page, (P)revious page, (E)xit" << std::endl;
            char option;
            std::cin >> option;

            if (option == 'N' || option == 'n') {
                if (currentPage < totalPages) {
                    currentPage++;
                } else {
                    std::cout << "Already on the last page." << std::endl;
                }
            } else if (option == 'P' || option == 'p') {
                if (currentPage > 1) {
                    currentPage--;
                } else {
                    std::cout << "Already on the first page." << std::endl;
                }
            } else if (option == 'E' || option == 'e') {
                break;
            } else {
                std::cout << "Invalid option. Please try again." << std::endl;
            }
        }
    }
}

void UpdateRecord(Student records[], int numRecords) {
    if (numRecords == 0) {
        std::cout << "No records found." << std::endl;
    } else {
        int recordNumber;
        std::cout << "Enter the number of the record to update: ";
        std::cin >> recordNumber;

        if (recordNumber >= 1 && recordNumber <= numRecords) {
            Student& student = records[recordNumber - 1];
            std::cout << "Updating Record " << recordNumber << std::endl;
            GetStudentInformation(student);
            std::cout << "Record " << recordNumber << " updated successfully." << std::endl;
        } else {
            std::cout << "Invalid record number. Please try again." << std::endl;
        }
    }
}

void DeleteRecord(Student records[], int& numRecords) {
    if (numRecords == 0) {
        std::cout << "No records found." << std::endl;
    } else {
        int recordNumber;
        std::cout << "Enter the number of the record to delete: ";
        std::cin >> recordNumber;

        if (recordNumber >= 1 && recordNumber <= numRecords) {
            int index = recordNumber - 1;
            for (int i = index; i < numRecords - 1; i++) {
                records[i] = records[i + 1];
            }
            numRecords--;
            std::cout << "Record " << recordNumber << " deleted successfully." << std::endl;
        } else {
            std::cout << "Invalid record number. Please try again." << std::endl;
        }
    }
    std::cout << "Press Enter to continue...";
    ClearInputBuffer();
    std::cin.get();
}

void GetStudentInformation(Student& student) {
    std::cout << "Enter First Name: ";
    std::getline(std::cin >> std::ws, student.firstName);

    std::cout << "Enter Last Name: ";
    std::getline(std::cin >> std::ws, student.lastName);

    std::cout << "Enter Student Year: ";
    std::string yearInput;
    std::getline(std::cin >> std::ws, yearInput);
    student.year = std::stoi(yearInput);

    std::cout << "Enter Section: ";
    std::getline(std::cin >> std::ws, student.section);

    std::cout << "Enter Course: ";
    std::getline(std::cin >> std::ws, student.course);
}

void PrintStudentInformation(const Student& student) {
    PrintBorder();
    std::cout << "Student Information:" << std::endl;
    std::cout << "First Name: " << student.firstName << std::endl;
    std::cout << "Last Name: " << student.lastName << std::endl;
    std::cout << "Student Year: " << student.year << std::endl;
    std::cout << "Section: " << student.section << std::endl;
    std::cout << "Course: " << student.course << std::endl;
    PrintBorder();
}


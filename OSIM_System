#include <iostream>
#include <fstream>
#include <string>
#include <ctime>

using namespace std;

// Task structure to hold task data
struct Task {
    string taskTitle;
    string taskDescription;
    string assignee;
    string status; // Status: Created, Assigned, InProgress, Completed, Expired
    string dueDate; // Expiry date
};

void taskMenu(const string& username, const string& taskDetails);
void completeTask(const string& username, const string& taskDetails);
void extendTime(const string& username, const string& taskDetails);
void markTaskAsCompleted(const std::string& taskDetails);
void backToMenu();
void createNotification(const string& sender, const string& type, const string& message);











void reverseString(string& str) {
    int n = str.length();
    for (int i = 0; i < n / 2; ++i) {
        swap(str[i], str[n - i - 1]);
    }
}

void toUpperCase(string& str) {
    for (char& c : str) {
        c = toupper(c);
    }
}










// === Custom Hash Function ===
string simpleHash(string password) {
    string hash = "";
    for (char c : password) {
        hash += to_string((int)c + 7); // Simple shift hash
    }
    return hash;
}

// === Role Classes ===
class User {
protected:
    string username, password, role;
public:
    User(string u, string p, string r) : username(u), password(p), role(r) {}
    virtual void displayRole() { cout << "Role: " << role << endl; }
    string getRole() { return role; }
    string getUsername() { return username; }
};

// Derived classes (can add more features later)
class Junior : public User { public: Junior(string u, string p) : User(u, p, "Junior") {} };
class Employee : public Junior { public: Employee(string u, string p) : Junior(u, p) { role = "Employee"; } };
class Manager : public Employee { public: Manager(string u, string p) : Employee(u, p) { role = "Manager"; } };
class Director : public Manager { public: Director(string u, string p) : Manager(u, p) { role = "Director"; } };
class Executive : public Director { public: Executive(string u, string p) : Director(u, p) { role = "Executive"; } };

int getRoleRank(string role) {
    if (role == "Junior") return 1;
    if (role == "Employee") return 2;
    if (role == "Manager") return 3;
    if (role == "Director") return 4;
    if (role == "Executive") return 5;
    return 0;
}

bool canAssignTo(string fromRole, string toRole) {
    return getRoleRank(fromRole) > getRoleRank(toRole);
}

void logAudit(const string& action, const string& username, const string& details = "") {
    ofstream auditFile("audit.txt", ios::app);
    if (auditFile.is_open()) {
        time_t now = time(0);  // Get current time
        char buf[20];
        strftime(buf, sizeof(buf), "%Y-%m-%d %H:%M:%S", localtime(&now)); // Format time

        auditFile << "[" << buf << "] "
                  << "Action: " << action
                  << " | User: " << username;
        if (!details.empty()) {
            auditFile << " | Details: " << details;
        }
        auditFile << endl;
        auditFile.close();
    } else {
        cout << "Error: Could not open audit file.\n";
    }
}

// === Policy Engine ===
class PolicyEngine {
public:
    static bool checkPermission(string role, string action) {
        // Simulated permission logic
        if (role == "Junior" && action == "delegate_task") return false;
        return true; // Default allow for others
    }
};

// === OTP Generator ===
string generateOTP() {
    srand(time(0));
    int otp = rand() % 10000;
    char buffer[6]; // 4 digits + '\0' = 5, so size 6 is safe
    sprintf(buffer, "%04d", otp);
    return string(buffer);
}

// === ASCII Menu ===
void displayMenu() {
    cout << "\n========================================\n";
    cout << "|         OSIM Login/Register          |\n";
    cout << "========================================\n";
    cout << "1. Register\n";
    cout << "2. Login\n";
    cout << "3. Exit\n";
    cout << "Choose an option: ";
}

// === Register User ===
void registerUser() {
    string username, password, role;

    // Get user input
    cout << "Enter username: ";
    cin >> username;

    // Check if username already exists
    ifstream infile("users.txt");
    string existingUser, existingPass, existingRole;
    bool userExists = false;
    while (infile >> existingUser >> existingPass >> existingRole) {
        if (existingUser == username) {
            userExists = true;
            break;
        }
    }
    infile.close();

    if (userExists) {
        cout << "Error: Username already exists. Please try another.\n";
        return;
    }

    cout << "Enter password: ";
    cin >> password;



    cout << "Enter role (Junior/Employee/Manager/Director/Executive): ";
    cin >> role;

    // Validate role input
    if (role != "Junior" && role != "Employee" && role != "Manager" &&
        role != "Director" && role != "Executive") {
        cout << "Error: Invalid role entered. Please try again.\n";
        return;
    }

    // All validations passed — proceed with registration
    ofstream file("users.txt", ios::app);
    file << username << " " << simpleHash(password) << " " << role << endl;
    file.close();

    cout << "User registered successfully!\n";
}

// === Login with OTP ===

string loginUser() {
    string username, password, role, file_user, file_pass, file_role;
    bool found = false;
    int attempts = 0;

    while (attempts < 3) {
        cout << "Enter username: ";
        cin >> username;
        cout << "Enter password: ";
        cin >> password;

        ifstream file("users.txt");
        while (file >> file_user >> file_pass >> file_role) {
            if (file_user == username && file_pass == simpleHash(password)) {
                role = file_role;
                found = true;
                break;
            }
        }
        file.close();

        if (found) {
            string otp = generateOTP();
            cout << "\n--- OTP sent via secure messaging ---\n";
            cout << "[Demo Message]: Your OTP is: " << otp << endl;

            time_t start = time(0);  // OTP timer start
            string userOTP;
            cout << "Enter OTP (valid for 10 seconds): ";
            cin >> userOTP;
            time_t end = time(0);  // OTP timer end

            double elapsed = difftime(end, start);

            if (elapsed > 10) {
                cout << "OTP expired. Access denied.\n";
                logAudit("Failed login (OTP expired)", username);
                return "";
            }
            if (userOTP == otp) {
                cout << "Login successful! Role: " << role << endl;
                logAudit("Successful login", username, "Role: " + role);
                return username + ":" + role;
            } else {
                cout << "Incorrect OTP. Access denied.\n";
                logAudit("Failed login (Incorrect OTP)", username);
                return "";
            }
        } else {
            attempts++;
            cout << "Incorrect credentials. Attempts left: " << (3 - attempts) << endl;
        }
    }
    cout << "Too many failed attempts. Access denied.\n";
    logAudit("Failed login (Too many attempts)", username);
    return "";
}










void createTask(const string& title, const string& description, const string& username) {
    // Get the current time
    time_t currentTime = time(0);
    char timeBuf[20];
    strftime(timeBuf, sizeof(timeBuf), "%Y-%m-%d %H:%M:%S", localtime(&currentTime));
    string timeOfCreation = timeBuf;

    // Clear input buffer before using getline
    cin.ignore();

    // Ask user to input the deadline
    string deadline;
    cout << "Enter the deadline for the task (format: YYYY-MM-DD HH:MM:SS): ";
    getline(cin, deadline);

    // Ask user to input the priority
    string priority;
    cout << "Enter task priority (High, Medium, Low): ";
    getline(cin, priority);

    // Check if the priority input is valid
    if (priority != "High" && priority != "Medium" && priority != "Low") {
        cout << "Invalid priority entered. Please enter High, Medium, or Low." << endl;
        return;
    }

    // Save the task to a file
    ofstream taskFile("task.txt", ios::app);
    if (taskFile.is_open()) {
        taskFile << "Task Title: " << title << endl;
        taskFile << "Task Description: " << description << endl;
        taskFile << "Created by: " << username << endl;
        taskFile << "Time of Creation: " << timeOfCreation << endl;
        taskFile << "Deadline: " << deadline << endl;
        taskFile << "Priority: " << priority << "\n"<< endl;

        taskFile.close();
    } else {
        cout << "Error: Could not open file to save task." << endl;
    }

    // Log the task creation in the audit log
    string taskDetails = "Task created | Title: " + title + " | Description: " + description + " | Deadline: " + deadline + " | Priority: " + priority;
    logAudit("Created New Task", username, taskDetails);
}

void displayTasks() {
    // Open the task file
    ifstream taskFile("task.txt");
    string line;

    if (taskFile.is_open()) {
        while (getline(taskFile, line)) {
            cout << line << endl;
        }
        taskFile.close();
    } else {
        cout << "Error: Could not open task file." << endl;
    }
}

void selectTask(const string& username) {
    ifstream taskFile("task.txt");
    string line;
    int taskNumber = 1;

    if (taskFile.is_open()) {
        cout << "\n===== Select a Task =====\n";
        
        // Display only the task names (titles) that are not marked as "Completed"
        while (getline(taskFile, line)) {
            string taskTitle = line;
            string taskDescription, taskCreator, taskTime, taskStatus;
            getline(taskFile, taskDescription); // Skip description
            getline(taskFile, taskCreator); // Skip creator
            getline(taskFile, taskTime); // Skip time of creation
            getline(taskFile, taskStatus); // Read status

            if (taskStatus != "Completed") {
                cout << taskNumber << ". " << taskTitle << endl; // Display task title only
            }

            // Skip the separator line
            getline(taskFile, line);
            taskNumber++;
        }
        taskFile.close();

        // Ask user to select a task
        int taskChoice;
        cout << "Enter the task number to select: ";
        cin >> taskChoice;

        // Reopen file to read selected task
        taskFile.open("task.txt");
        int currentTask = 1;
        while (getline(taskFile, line)) {
            if (currentTask == taskChoice) {
                string taskDetails = line; // Task title is the first line
                getline(taskFile, line); // Skip description
                getline(taskFile, line); // Skip creator
                getline(taskFile, line); // Skip time of creation
                string taskStatus;
                getline(taskFile, taskStatus); // Read status
                if (taskStatus != "Completed") {
                    taskMenu(username, taskDetails); // Call task menu with selected task
                } else {
                    cout << "Task is already completed and cannot be selected.\n";
                }
                break;
            }
            currentTask++;
        }

        if (currentTask != taskChoice) {
            cout << "Invalid task number.\n";
        }

        taskFile.close();
    } else {
        cout << "Error: Could not open task.txt.\n";
    }
}

void taskMenu(const string& username, const string& taskDetails) {
    int choice;
    cout << "\nTask: " << taskDetails << endl;
    cout << "1. Complete Task\n";
    cout << "2. Extend Time\n";
    cout << "3. Back\n";
    cout << "Choose an option: ";
    cin >> choice;

    switch (choice) {
        case 1:
            markTaskAsCompleted(taskDetails); // Mark the task as completed
            break;
        case 2:
            extendTime(username, taskDetails);
            break;
        case 3:
            
            break;
        default:
            cout << "Invalid option.\n";
    }
}

void completeTask(const string& username, const string& taskDetails) {
    cout << "Task '" << taskDetails << "' marked as completed.\n";

    // Log audit action
    logAudit("Completed task", username, "Task: " + taskDetails);
}

void extendTime(const string& username, const string& taskDetails) {
    string newDueDate;
    cout << "Enter new due date/time for the task '" << taskDetails << "': ";
    cin.ignore(); // To clear the buffer before getline
    getline(cin, newDueDate);

    cout << "Task '" << taskDetails << "' time extended to: " << newDueDate << endl;

    // Log audit action
    logAudit("Extended task time", username, "Task: " + taskDetails + " New Due Date: " + newDueDate);
}

// Function to mark a task as completed
void markTaskAsCompleted(const std::string& taskDetails) {
    std::ifstream taskFile("task.txt");
    std::ofstream tempFile("temp.txt");
    std::string line;
    bool taskFound = false;

    if (taskFile.is_open() && tempFile.is_open()) {
        while (std::getline(taskFile, line)) {
            std::string taskTitle = line;
            std::string taskDescription, taskCreator, taskTime, taskStatus;
            std::getline(taskFile, taskDescription);
            std::getline(taskFile, taskCreator);
            std::getline(taskFile, taskTime);
            std::getline(taskFile, taskStatus);

            if (taskTitle == taskDetails) {
                // Mark task as completed
                taskStatus = "Completed";
            }

            // Write the task (whether updated or not)
            tempFile << taskTitle << std::endl;
            tempFile << taskDescription << std::endl;
            tempFile << taskCreator << std::endl;
            tempFile << taskTime << std::endl;
            tempFile << taskStatus << std::endl;
        }

        taskFile.close();
        tempFile.close();

        // Replace original file with the updated file
        std::remove("task.txt");
        std::rename("temp.txt", "task.txt");

        std::cout << "Task marked as completed.\n";
    } else {
        std::cout << "Error: Could not open task files.\n";
    }
}

string getCurrentTime() {
    // Get the current time as a string in a readable format
    time_t now = time(0);  // Get the current time
    struct tm* timeinfo = localtime(&now);  // Convert to local time structure
    char buffer[80];
    strftime(buffer, sizeof(buffer), "%Y-%m-%d %H:%M:%S", timeinfo);  // Format the time as a string
    return string(buffer);  // Return the formatted time as a string
}
void backToMenu() {
    cout << "\nPress Enter to go back to the menu...";
    cin.ignore();
    cin.get();
}

void checkAndExpireTasks() {
    ifstream taskFile("task.txt");
    ofstream tempFile("temp.txt");

    if (!taskFile.is_open() || !tempFile.is_open()) {
        cout << "Error opening task files!" << endl;
        return;
    }

    string line;
    bool keepTask = true;
    string taskTitle, deadline;
    
    while (getline(taskFile, line)) {
        if (line.find("Task Title: ") != string::npos) {
            taskTitle = line.substr(12); // Save title for audit
            keepTask = true;
        }
        if (line.find("Deadline: ") != string::npos) {
            deadline = line.substr(10);

            // Manually parse the deadline string in the format YYYY-MM-DD HH:MM:SS
            struct tm tmDeadline = {};
            int year, month, day, hour, minute, second;
            sscanf(deadline.c_str(), "%d-%d-%d %d:%d:%d", &year, &month, &day, &hour, &minute, &second);
            
            // Fill the tm structure with the parsed values
            tmDeadline.tm_year = year - 1900;  // tm_year is years since 1900
            tmDeadline.tm_mon = month - 1;     // tm_mon is 0-based
            tmDeadline.tm_mday = day;
            tmDeadline.tm_hour = hour;
            tmDeadline.tm_min = minute;
            tmDeadline.tm_sec = second;
            tmDeadline.tm_isdst = -1;  // Let mktime decide if daylight saving time is in effect

            time_t deadlineTime = mktime(&tmDeadline);
            time_t currentTime = time(nullptr);

            if (currentTime > deadlineTime) {
                // Expired
                keepTask = false;
                logAudit("Task expired and removed", "SYSTEM", "Task Title: " + taskTitle);
                cout << "Task '" << taskTitle << "' expired and removed!" << endl;
            }
        }

        if (keepTask) {
            tempFile << line << endl;
        }

        if (line == "-----------------------------------") {
            keepTask = true;
        }
    }

    taskFile.close();
    tempFile.close();

    // Replace old task file with new one
    remove("task.txt");
    rename("temp.txt", "task.txt");
}

void viewAuditLogs() {
    ifstream auditFile("audit.txt");

    if (auditFile.is_open()) {
        cout << "\n========== Audit Logs ==========\n";

        string line;
        bool isEmpty = true;
        while (getline(auditFile, line)) {
            cout << line << endl;
            isEmpty = false;
        }

        if (isEmpty) {
            cout << "No audit records found.\n";
        }

        cout << "=================================\n";
        auditFile.close();
    } else {
        cout << "Error: Could not open audit.txt.\n";
    }

    backToMenu(); // Return to the menu after viewing logs
}

void removeTask(const string& username) {
    string targetTitle;
    cout << "Enter the title of the task you want to remove: ";
    cin.ignore();
    getline(cin, targetTitle);

    ifstream taskFile("task.txt");
    if (!taskFile.is_open()) {
        cout << "Error: Could not open task.txt.\n";
        return;
    }

    ofstream tempFile("task_temp.txt");
    if (!tempFile.is_open()) {
        cout << "Error: Could not create temporary file.\n";
        taskFile.close();
        return;
    }

    string line;
    bool found = false;
    bool skipTask = false;

    while (getline(taskFile, line)) {
        // If a new task starts
        if (line.find("Task Title: ") != string::npos) {
            // Check if this task is the one to remove
            if (line.find("Task Title: " + targetTitle) != string::npos) {
                found = true;
                skipTask = true; // Start skipping lines
                continue; // Skip writing this title line
            } else {
                skipTask = false; // Start writing this task
            }
        }

        // If not skipping, write the line to temp file
        if (!skipTask) {
            tempFile << line << endl;
        }
    }

    taskFile.close();
    tempFile.close();

    if (found) {
        remove("task.txt");
        rename("task_temp.txt", "task.txt");
        cout << "Task '" << targetTitle << "' has been removed successfully.\n";

        // Log the deletion
        string detail = "Removed task with title: " + targetTitle;
        logAudit("Removed Task", username, detail);
    } else {
        cout << "Task with title '" << targetTitle << "' not found.\n";
        remove("task_temp.txt"); // Clean up
    }

    backToMenu(); // After removing, go back to menu
}

void autoExpireTasks(const string& username) {
    ifstream taskFile("task.txt");
    if (!taskFile.is_open()) {
        cout << "Error: Could not open task.txt.\n";
        return;
    }

    ofstream tempFile("task_temp.txt");
    if (!tempFile.is_open()) {
        cout << "Error: Could not create temporary file.\n";
        return;
    }

    string line;
    string taskData = "";
    bool taskExpired = false;

    while (getline(taskFile, line)) {
        taskData += line + "\n";
        
        if (line.find("-----------------------------------") != string::npos) {
            // End of one task
            size_t pos = taskData.find("Deadline: ");
            bool shouldSave = true;

            if (pos != string::npos) {
                string deadline = taskData.substr(pos + 10, 19); // Get "YYYY-MM-DD HH:MM:SS"

                struct tm deadline_tm = {};
                int year, month, day, hour, minute, second;
                sscanf(deadline.c_str(), "%d-%d-%d %d:%d:%d", &year, &month, &day, &hour, &minute, &second);
                
                // Fill the tm structure with the parsed values
                deadline_tm.tm_year = year - 1900;  // tm_year is years since 1900
                deadline_tm.tm_mon = month - 1;     // tm_mon is 0-based
                deadline_tm.tm_mday = day;
                deadline_tm.tm_hour = hour;
                deadline_tm.tm_min = minute;
                deadline_tm.tm_sec = second;
                deadline_tm.tm_isdst = -1;  // Let mktime decide if daylight saving time is in effect

                time_t deadline_time = mktime(&deadline_tm);
                time_t now = time(0);

                if (difftime(deadline_time, now) < 0) {
                    // Deadline passed, don't save
                    shouldSave = false;
                    cout << "Task expired and removed: " << taskData.substr(0, taskData.find('\n')) << endl;
                    string detail = "Auto-expired task: " + taskData.substr(0, taskData.find('\n'));
                    logAudit("Task Auto-Expired", username, detail);
                    taskExpired = true;
                }
            }

            if (shouldSave) {
                tempFile << taskData;
            }

            taskData = ""; // Reset for next task
        }
    }

    taskFile.close();
    tempFile.close();

    remove("task.txt");
    rename("task_temp.txt", "task.txt");

    if (!taskExpired) {
        cout << "No tasks were expired.\n";
    }
}

void createNotification(const string& username, const string& type, const string& message) {
    // Open the notifications.txt file to append
    ofstream notifFile("notifications.txt", ios::app);
    if (notifFile.is_open()) {
        // Get the current time
        time_t now = time(0);  // Get the current time
        string timestamp = ctime(&now);
        timestamp.pop_back(); // Remove newline character

        // Write the notification to the file
        notifFile << "[" << timestamp << "] " 
                  << "Type: " << type << " | "
                  << "Message: " << message << " | "
                  << "Sent by: " << username << endl;

        notifFile.close();
        cout << "Notification sent and saved!\n";
    } else {
        cout << "Error: Could not open notifications file.\n";
    }
}

void readNotifications() {
    ifstream file("notifications.txt");
    if (!file.is_open()) {
        cout << "Error: Could not open notifications.txt.\n";
        return;
    }

    string line;
    cout << "\n=== Notifications ===\n";
    while (getline(file, line)) {
        cout << line << endl;
    }

    file.close();
}

// Simple encryption: reverse the message (replace with real one as needed)
string encryptMessage(const string& message) {
    string encrypted = message;
    
    // Manually reverse the string
    int n = encrypted.length();
    for (int i = 0; i < n / 2; ++i) {
        // Swap characters
        char temp = encrypted[i];
        encrypted[i] = encrypted[n - i - 1];
        encrypted[n - i - 1] = temp;
    }
    
    return encrypted;
}

string decryptMessage(const string& encrypted) {
    string decrypted = encrypted;
    
    // Manually reverse the string
    int n = decrypted.length();
    for (int i = 0; i < n / 2; ++i) {
        // Swap characters
        char temp = decrypted[i];
        decrypted[i] = decrypted[n - i - 1];
        decrypted[n - i - 1] = temp;
    }
    
    return decrypted;
}

void sendMessage(const string& username, const string& senderRole) {
    string receiverUsername, messageType, message;

    cin.ignore(); // Clear buffer
    cout << "Enter recipient username: ";
    getline(cin, receiverUsername);

    cout << "Enter message type (INFO, PRIVATE, ALERT): ";
    getline(cin, messageType);

    // Manually convert messageType to uppercase
    for (char& c : messageType) {
        c = toupper(c);  // Convert each character to uppercase
    }

    // Permission check
    if (messageType == "ALERT" && senderRole != "Director" && senderRole != "Executive") {
        cout << "You do not have permission to send ALERT messages.\n";
        return;
    }

    cout << "Enter your message: ";
    getline(cin, message);

    if (messageType == "PRIVATE") {
        message = encryptMessage(message);
    }

    // Create file name using std::string (no need for char array)
    string inboxFile = "inbox_" + receiverUsername + ".txt";

    ofstream file(inboxFile, ios::app);
    if (!file.is_open()) {
        cout << "Error: Cannot open " << inboxFile << "\n";
        return;
    }

    // Get current timestamp using <ctime>
    time_t now = time(0);  // Get the current time
    string timestamp = ctime(&now);  // Convert to string
    timestamp.pop_back(); // Remove trailing newline

    // Write to file
    file << "Sender: " << username << endl;
    file << "Type: " << messageType << endl;
    file << "Timestamp: " << timestamp << endl;
    file << "Message: " << message << "\n" << endl;
    file.close();

    cout << "Message sent successfully to " << receiverUsername << "!\n";
}

void readInbox(const string& username) {
    // Construct filename using std::string (no need for C-style strings)
    string inboxFile = "inbox_" + username + ".txt";

    ifstream file(inboxFile);
    if (!file.is_open()) {
        cout << "You have no messages yet.\n";
        return;
    }

    string line, sender, type, timestamp, message;
    bool hasMessage = false;

    cout << "\n=== Inbox for " << username << " ===\n";

    while (getline(file, line)) {
        if (line.find("Sender: ") == 0) {
            sender = line.substr(8);
        } else if (line.find("Type: ") == 0) {
            type = line.substr(6);
        } else if (line.find("Timestamp: ") == 0) {
            timestamp = line.substr(10);
        } else if (line.find("Message: ") == 0) {
            message = line.substr(9);
        } else if (line == "-----") {
            hasMessage = true;
            if (type == "PRIVATE") {
                message = decryptMessage(message);  // Assuming you have a decryption function
            }
            cout << "From: " << sender << "\nType: " << type << "\nTime: " << timestamp << "\nMessage: " << message << endl;
            cout << "-------------------------\n";
        }
    }

    file.close();

    if (!hasMessage) {
        cout << "You have no messages yet.\n";
    }
}

void assignTaskToUser(const string& assignerUsername, const string& assignerRole) {
    if (assignerRole != "Manager" && assignerRole != "Director" && assignerRole != "Executive") {
        cout << "You are not authorized to assign tasks.\n";
        return;
    }

    string taskName, taskDesc, assignee, ttl;
    cout << "Enter assignee username (Junior or Employee): ";
    cin >> assignee;
    cout << "Enter task title: ";
    cin.ignore(); getline(cin, taskName);
    cout << "Enter task description: ";
    getline(cin, taskDesc);
    cout << "Enter TTL (in minutes): ";
    cin >> ttl;

    ofstream fout("task_" + assignee + ".txt", ios::app);
    fout << taskName << "|" << taskDesc << "|" << assignerUsername << "|" << ttl << "|Assigned" << endl;
    fout.close();

    ofstream audit("audit.txt", ios::app);
    audit << "Task '" << taskName << "' assigned by " << assignerUsername << " to " << assignee << " with TTL: " << ttl << " mins.\n";
    audit.close();

    cout << "Task assigned successfully to " << assignee << ".\n";
}

void viewMyTasks(const string& username) {
    string filename = "task_" + username + ".txt";
    ifstream fin(filename);
    if (!fin.is_open()) {
        cout << "No tasks assigned to you.\n";
        return;
    }

    string line;
    bool changesMade = false;
    ofstream temp("temp.txt");

    while (getline(fin, line)) {
        string taskName, taskDesc, assignedBy, ttl, status;
        size_t pos = 0;

        // Parse task line
        pos = line.find("|"); taskName = line.substr(0, pos); line = line.substr(pos + 1);
        pos = line.find("|"); taskDesc = line.substr(0, pos); line = line.substr(pos + 1);
        pos = line.find("|"); assignedBy = line.substr(0, pos); line = line.substr(pos + 1);
        pos = line.find("|"); ttl = line.substr(0, pos); line = line.substr(pos + 1);
        status = line;

        if (status == "Completed") {
            temp << taskName << "|" << taskDesc << "|" << assignedBy << "|" << ttl << "|" << status << endl;
            continue; // skip completed
        }

        cout << "\nTask: " << taskName << "\nDescription: " << taskDesc << "\nAssigned By: " << assignedBy << "\nTTL: " << ttl << " mins\nStatus: " << status << endl;
        cout << "[1] Mark as Completed\n[2] Request Extension\n[3] Skip\nChoice: ";
        int choice;
        cin >> choice;

        if (choice == 1) {
            status = "Completed";
            cout << "Marked as completed.\n";

            ofstream audit("audit.txt", ios::app);
            audit << "User '" << username << "' completed task '" << taskName << "'.\n";
            audit.close();
        } else if (choice == 2) {
            cout << "Enter new TTL in minutes: ";
            cin >> ttl;
            cout << "Extension requested.\n";

            ofstream audit("audit.txt", ios::app);
            audit << "User '" << username << "' requested extension for task '" << taskName << "' to TTL: " << ttl << " mins.\n";
            audit.close();
        } else {
            cout << "Skipped.\n";
        }

        temp << taskName << "|" << taskDesc << "|" << assignedBy << "|" << ttl << "|" << status << endl;
        changesMade = true;
    }

    fin.close();
    temp.close();

    if (changesMade) {
        remove(filename.c_str());
        rename("temp.txt", filename.c_str());
    } else {
        remove("temp.txt");
    }
}









void juniorMenu(const string& username, const string& role) {
    int choice;
    while (true) { // Keep the menu active until the user exits
        cout << "\n[Junior Task Menu]\n";
        cout << "1. Select Assigned Tasks\n";
        cout << "2. Read Global Message\n";
        cout << "3. Send a secure message\n";
        cout << "4. Read my inbox\n";
        cout << "5. View personal assigned tasks\n";
        cout << "6. Exit\n"; // Option to exit the menu
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
            case 1:
                selectTask(username);
                break;
            case 2:
                readNotifications();
                break;
            case 3:
                sendMessage(username, role);
                break;
            case 4:
                readInbox(username);
                break;
            case 5:
                viewMyTasks(username);
                break;
            case 6:
                cout << "Exiting Junior Task Menu.\n";
                return; // Exit the menu
            default:
                cout << "Invalid option.\n";
        }
    }
}

void employeeMenu(const string& username, const string& role) {
    int choice;
    while (true) { // Keep the menu active until the user exits
        cout << "\n[Employee Task Menu]\n";
        cout << "1. View Assigned Tasks\n";
        cout << "2. Read Global Message\n";
        cout << "3. Send a secure message\n";
        cout << "4. Read my inbox\n";
        cout << "5. View personal assigned tasks\n";
        cout << "5. Exit\n"; // Option to exit the menu
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
            case 1:
                selectTask(username);
                break;
            case 2:
                readNotifications();
                break;
            case 3:
                sendMessage(username, role);
                break;
            case 4:
                readInbox(username);
                break;
            case 5:
                viewMyTasks(username);
                break;
            case 6:
                cout << "Exiting Employee Task Menu.\n";
                return; // Exit the menu
            default:
                cout << "Invalid option.\n";
        }
    }
}

void managerMenu(const string& username, const string& role) {
    int choice;
    while (true) { // Keep the menu active until the user exits
        cout << "\n[Manager Task Menu]\n";
        cout << "1. Create Task\n";
        cout << "2. Display Task\n";
        cout << "3. Global Message\n";
        cout << "4. Read Global Message\n";
        cout << "5. Send a secure message\n";
        cout << "6. Read my inbox\n";
        cout << "7. Exit\n"; // Option to exit the menu
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                string title, description;
                cin.ignore();  // Clears newline from input buffer
                cout << "Enter task title: ";
                getline(cin, title);
                cout << "Enter task description: ";
                getline(cin, description);
                createTask(title, description, username);
                break;
            }
            case 2:
                displayTasks();
                break;
            case 3: {
                string type, message; // Declare both here

                cout << "Enter notification type (WARNING or EMERGENCY): ";
                cin >> type;
                cin.ignore(); // clear the input buffer

                cout << "Enter notification message: ";
                getline(cin, message);

                createNotification(username, type, message);
                break;
            }
            case 4:
                readNotifications();
                break;
            case 5:
                sendMessage(username, role);
                break;
            case 6:
                readInbox(username);
                break;
            case 7:
                cout << "Exiting Manager Task Menu.\n";
                return; // Exit the menu
            default:
                cout << "Invalid option.\n";
        }
    }
}

void directorMenu(const string& username, const string& role) {
    int choice;
    while (true) { // Keep the menu active until the user exits
        cout << "\n[Director Task Menu]\n";
        cout << "1. Create Task\n";
        cout << "2. View All Tasks and Statuses\n";
        cout << "3. Force Expire Task\n";
        cout << "4. Generate Audit Report\n";
        cout << "5. Global Message\n";
        cout << "6. Read Global Message\n";
        cout << "7. Send a secure message\n";
        cout << "8. Read my inbox\n";
        cout << "9. Assign task\n";
        cout << "10. Exit\n"; // Option to exit the menu
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                string title, description;
                cin.ignore();  // Clears newline from input buffer
                cout << "Enter task title: ";
                getline(cin, title);
                cout << "Enter task description: ";
                getline(cin, description);
                createTask(title, description, username);
                break;
            }
            case 2:
                displayTasks();
                break;
            case 3:
                removeTask(username);
                break;
            case 4:
                viewAuditLogs();
                break;
            case 5: {
                string type, message; // Declare both here

                cout << "Enter notification type (WARNING or EMERGENCY): ";
                cin >> type;
                cin.ignore(); // clear the input buffer

                cout << "Enter notification message: ";
                getline(cin, message);

                createNotification(username, type, message);
                break;
            }
            case 6:
                readNotifications();
                break;
            case 7:
                sendMessage(username, role);
                break;
            case 8:
                readInbox(username);
                break;
            case 9:
                assignTaskToUser(username, role);
                break;
            case 10:
                cout << "Exiting Director Task Menu.\n";
                return; // Exit the menu
            default:
                cout << "Invalid option.\n";
        }
    }
}

void executiveMenu(const string& username, const string& role) {
    int choice;
    while (true) { // Keep the menu active until the user exits
        cout << "\n[Executive Task Menu]\n";
        cout << "1. Create Task\n";
        cout << "2. View All Tasks and Their Status\n";
        cout << "3. Expire or Delete Task\n";
        cout << "4. Generate Audit Report\n";
        cout << "5. Global Message\n";
        cout << "6. Read Global Message\n";
        cout << "7. Send a secure message\n";
        cout << "8. Read my inbox\n";
        cout << "9. Assign task\n";
        cout << "10. Exit\n"; // Option to exit the menu
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                string title, description;
                cin.ignore();  // Clears newline from input buffer
                cout << "Enter task title: ";
                getline(cin, title);
                cout << "Enter task description: ";
                getline(cin, description);
                createTask(title, description, username);
                break;
            }
            case 2:
                displayTasks();
                break;
            case 3:
                removeTask(username);
                break;
            case 4:
                viewAuditLogs();
                break;
            case 5: {
                string type, message; // Declare both here

                cout << "Enter notification type (WARNING or EMERGENCY): ";
                cin >> type;
                cin.ignore(); // clear the input buffer

                cout << "Enter notification message: ";
                getline(cin, message);

                createNotification(username, type, message);
                break;
            }
            case 6:
                readNotifications();
                break;
            case 7:
                sendMessage(username, role);
                break;
            case 8:
                readInbox(username);
                break;
            case 9:
                assignTaskToUser(username, role);
                break;
            case 10:
                cout << "Exiting Executive Task Menu.\n";
                return; // Exit the menu
            default:
                cout << "Invalid option.\n";
        }
    }
}










// === Main ===
int main() {
    int choice;
    
    string currentUsername;
    string currentRole;
    
    while (true) {
        displayMenu();
        cin >> choice;
        switch (choice) {
            case 1: registerUser(); break;
            case 2: {
    string result = loginUser();
    if (!result.empty()) {
        // FIX: store into currentUsername and currentRole
        currentUsername = result.substr(0, result.find(":"));
        currentRole = result.substr(result.find(":") + 1);

        if (currentRole == "Junior") juniorMenu(currentUsername, currentRole);
        else if (currentRole == "Employee") employeeMenu(currentUsername, currentRole);
        else if (currentRole == "Manager") managerMenu(currentUsername, currentRole);
        else if (currentRole == "Director") directorMenu(currentUsername, currentRole);
        else if (currentRole == "Executive") executiveMenu(currentUsername, currentRole);
    }
    break;
}


            case 3: cout << "Exiting...\n"; return 0;
            default: cout << "Invalid choice.\n";
        }
    }
    return 0;
}

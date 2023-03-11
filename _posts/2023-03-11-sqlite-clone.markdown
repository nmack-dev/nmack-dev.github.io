---
layout: post
title: "Sqlite Clone - My adventure into DB programming"
---

# The Why
I found myself interested in learning about the internals of a database. Not coming from a CS background, I had no prior knowledge. So naturally, I decided to build one myself using a tutorial, and C++ as my languge of choice.

# The Repl
It all starts with a Repl, aka a read-execute-print loop.
```cpp
int main()
{
    Table* table = new Table();
    std::string input_string;

    while (true) {
        std::cout << "db > ";
        std::getline(std::cin, input_string);

        if (InputHandler::IsMetaCommand(input_string)) {
            switch (InputHandler::DoMetaCommand(input_string)) {
                case META_COMMAND_SUCCESS:
                    continue;
                case META_COMMAND_UNRECOGNIZED:
                    std::cout << "Unrecognized command " << input_string << '.' << std::endl;
                    continue;
            }
        } else {
            Statement statement;

            switch (InputHandler::PrepareStatement(input_string, statement)) {
                case PREPARE_SUCCESS:
                    break;
                case PREPARE_SYNTAX_ERROR:
                    std::cout << "Statement Syntax Error: " << input_string << '.' << std::endl;
                    continue;
                case PREPARE_UNRECOGNIZED_STATEMENT:
                    std::cout << "Unrecognized keyword: " << input_string << '.' << std::endl;
                    continue;
            }

            // TODO: Handle InputHanlder failure cases during statement execution
            switch (statement.type) {
                case STATEMENT_INSERT:
                    InputHandler::ExecuteInsert(statement, table);
                    break;
                case STATEMENT_SELECT:
                    InputHandler::ExecuteSelect(statement, table);
                    break;
            }
        }
    }

    return 0;
}
``` 

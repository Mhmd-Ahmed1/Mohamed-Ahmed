Explain of Code: 
1-Variables:-1
int charClass;
char lexeme[100]; 
char nextChar; 
int lexLen; 
int toke; 
int nextToken; 
string input; 
int inputPos = 0;


2-class definition:

#define LETTER 0
#define DIGIT 1
#define UNKNOWN 99


3-tokens identifier

#define INT_LIT 10
#define IDENT 11
#define ASSIGN_OP 20
#define ADD_OP 21
#define SUB_OP 22
#define MULT_OP 23
#define DIV_OP 24
#define LEFT_PAREN 25
#define RIGHT_PAREN 26


4-Lookup function:

int lookup(char ch) {
    switch (ch) {
        case '(': addChar(); nextToken = LEFT_PAREN; break;
        case ')': addChar(); nextToken = RIGHT_PAREN; break;
        case '+': addChar(); nextToken = ADD_OP; break;
        case '-': addChar(); nextToken = SUB_OP; break;
        case '*': addChar(); nextToken = MULT_OP; break;
        case '/': addChar(); nextToken = DIV_OP; break;
        case '=': addChar(); nextToken = ASSIGN_OP; break; 
        default: addChar(); nextToken = UNKNOWN; break; 
    }
    return nextToken;
}


5-Addchar function

void addChar() {
    if (lexLen <= 98) { 
        lexeme[lexLen++] = nextChar;
        lexeme[lexLen] = '\0'; 
    } else {
        cout << "Error - lexeme is too long" << endl;
    }
}


6-GetChar Function:

void getChar() {
    if (inputPos < input.length()) {
        nextChar = input[inputPos++]; 
        if (isalpha(nextChar)) {
            charClass = LETTER; 
        } else if (isdigit(nextChar)) {
            charClass = DIGIT; 
        } else {
            charClass = UNKNOWN; 
        }
    } else {
        charClass = EOF; 
    }
}


7-skip blank function:

void getNonBlank() {
    while (isspace(nextChar)) { 
        getChar(); 
    }
}


8-lex Function:

lex: This function is the core of the lexical analyzer:
If the character is a letter, it keeps reading subsequent letters and digits to form an identifier, assigning the token IDENT.
If the character is a digit, it reads subsequent digits to form an integer literal, assigning the token INT_LIT.
If the character is an operator or parenthesis, it looks it up using the lookup function and assigns the corresponding token.
When it reaches the end of the input, it assigns the token EOF and outputs the lexeme as "EOF".

Code:
int lex() {
    lexLen = 0; 
    getNonBlank();
    switch (charClass) {
        case LETTER:
            addChar();
            getChar();
            while (charClass == LETTER || charClass == DIGIT) { 
                addChar();
                getChar();
            }
            nextToken = IDENT;
            break;
        
        case DIGIT:
            addChar();
            getChar();
            while (charClass == DIGIT) { 
                addChar();
                getChar();
            }
            nextToken = INT_LIT;
            break;
        
        case UNKNOWN:
            lookup(nextChar); 
            getChar();
            break;
        
        case EOF:
            nextToken = EOF;
            strcpy(lexeme, "EOF");
            break;
    }

    cout << "Next token is: " << nextToken << ", Next lexeme is " << lexeme << endl;
    return nextToken;
}

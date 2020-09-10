# MIPS-PROCESSOR
Allows an hexadecimal instruction to be converted and executed with the ability to print out the changes
#include <iostream>
#include <cmath>
using namespace std;

//takes users hexadecimal number and converts to binary to be used throughout the program.
string hex2bin(string hexnum) {
	string binnumbers[16] = { "0000","0001","0010","0011","0100","0101","0110","0111","1000","1001","1010","1011","1100","1101","1110","1111" };
	string binnum = "";

	for (long int i = 0; i < hexnum.length(); i++) {
		if (int(hexnum[i]) > 57) {
			binnum.append(binnumbers[int(hexnum[i]) - 55]);
		}
		else {
			binnum.append(binnumbers[int(hexnum[i]) - 48]);
		}
	}

	return binnum;
}

//gets values for a certian number range in the binary number and if it doesnt have 32 bits add four 0s at the end until it does.
string getvalue(string binnum, int start, int end) {
	string value = ""; if (binnum.length() != 32) {
		for (int i = binnum.length(); i < 32; i += 4) {
			binnum.append("0000");
		}
	}
	for (int i = start; i <= end; i++) { value.push_back(binnum[i]); }
	return value;
}

//converts binary number to decimal for registers, op and operations.
int bin2deci(string binint) {
	int decinum = 0; long int size = binint.length();
	for (long int i = 1; i <= size; i++) {

		if (binint[i - 1] == '1') { decinum += pow(2, size - i); }
	}
	return decinum;
}






int main() {
	int reg[16];
	for (int i = 0; i < 16; i++) {
		reg[i] = i;
	} 
	string user;
	while (user != "stop") {
		cout << "What is your hexadecimal number? (type 'stop' when done and 'revise' when you want to see register history)" << endl; cin >> user;
		if (user == "stop") {
			break;
		}
		if (user == "revise") {
			for (int i = 0; i < 16; i++) {
				cout << "$" << i << " = " << reg[i] << endl;
			}
		}
		else {
			//methods to get op,rs and rt
			string binnum = hex2bin(user);  int op = bin2deci(getvalue(binnum, 0, 5));
			int rs = bin2deci(getvalue(binnum, 6, 10));  int rt = bin2deci(getvalue(binnum, 11, 15));

			//Switch statements for Rformat
			if (op == 0) {
				int rd = bin2deci(getvalue(binnum, 16, 20)); int operation = bin2deci(getvalue(binnum, 26, 31));

				switch (operation) {
				case 36: reg[rd] = (reg[rs] == 1 && reg[rt] == 1) ? 1 : 18; cout << "and $" << rd << " $" << rs << " $" << rt << " " << reg[rd] << endl; cout << binnum << endl;
					 for (int i = 0; i < 16; i++) {cout << "$" << i << " = " << reg[i] << endl;} break;

				case 37: reg[rd] = (reg[rs] == 1 || reg[rt] == 1) ? 1 : 251; cout << "or $" << rd << " $" << rs << " $" << rt << " " << reg[rd] << endl; cout << binnum << endl;
					 for (int i = 0; i < 16; i++) {cout << "$" << i << " = " << reg[i] << endl;} break;

				case 32: reg[rd] = reg[rs] + reg[rt]; cout << "add $" << rd << " $" << rs << " $" << rt << " "  << endl; cout << binnum << endl;
					 for (int i = 0; i < 16; i++) { cout << "$" << i << " = " << reg[i] << endl; } break;

				case 34: reg[rd] = reg[rs] - reg[rt]; cout << "sub $" << rd << " $" << rs << " $" << rt << " "  << endl; cout << binnum << endl;
					 for (int i = 0; i < 16; i++) { cout << "$" << i << " = " << reg[i] << endl; } break;


				}
			}else {

				int ivalue = bin2deci(getvalue(binnum, 16, 31));
				//switch statements for Iformat
				switch (op) {
				case 35: reg[rt] = *(reg + rs + ivalue); cout << "lw $" << rt << " " << ivalue<< " ($" << rs << ")"<< endl; cout << binnum << endl;
					 for (int i = 0; i < 16; i++) { cout << "$" << i << " = " << reg[i] << endl; } break;

				case 43: *(reg + rs + ivalue) = reg[rt]; cout << "sw $" << rt << " " << ivalue<< " ($" << rs << ")"<< endl; cout << binnum << endl;
					 for (int i = 0; i < 16; i++) { cout << "$" << i << " = " << reg[i] << endl; } break;

				case 6: if (reg[rs] == reg[rt]) { cout << "beq $" << ivalue << "($" << rs << ") " << "Branching with offset: " << ivalue << endl; cout << binnum << endl; 
					    for (int i = 0; i < 16; i++) { cout << "$" << i << " = " << reg[i] << endl; } } break;

				case 5: if (reg[rs] != reg[rt]) { cout << "bne $" << ivalue << "($" << rs << ") " << "Branching with offset: " << ivalue << endl; cout << binnum << endl;
					   for (int i = 0; i < 16; i++) { cout << "$" << i << " = " << reg[i] << endl; } } break;

				case 8: reg[rt] = reg[rs] + ivalue; cout << "addi $" << rt << " "  << "$" << rs << " " << ivalue << endl; cout << binnum << endl;
					 for (int i = 0; i < 16; i++) { cout << "$" << i << " = " << reg[i] << endl; } break;

				}

			}
      
      
		}


	}

}

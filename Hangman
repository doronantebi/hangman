import string
import os.path

HANGMAN_ASCII_ART = """

    _    _
   | |  | |
   | |__| | __ _ _ __   __ _ _ __ ___   __ _ _ __
   |  __  |/ _' | '_ \ / _' | '_ ' _ \ / _' | '_
   | |  | | (_| | | | | (_| | | | | | | (_| | | | |
   |_|  |_|\__,_|_| |_|\__, |_| |_| |_|\__,_|_| |_|
                        __/ |
                       |___/
"""
MAX_TRIES = 6
LETTERS_GUESSED = []
picture1 = r"""
    x-------x
    """
picture2 = r"""
    x-------x
    |
    |
    |
    |
    |

 """
picture3 = r"""
    x-------x
    |       |
    |       0
    |
    |
    |
 """
picture4 = """
    x-------x
    |       |
    |       0
    |       |
    |
    |
 """
picture5 = r"""
    x-------x
    |       |
    |       0
    |      /|\
    |
    |

"""
picture6 = r"""
    x-------x
    |       |
    |       0
    |      /|\
    |      /
    |
"""
picture7 = r"""
    x-------x
    |       |
    |       0
    |      /|\
    |      / \
    | """
HANGMAN_PHOTOS = {6: picture1, 5: picture2, 4: picture3, 3: picture4, 2: picture5, 1: picture6, 0: picture7}


def intro_screen():
    """"Prints the intro screen for the game"""
    print(HANGMAN_ASCII_ART)
    print(MAX_TRIES)


def is_valid_input(letter_guessed):
    """checks whether the letter given by the user is legal
    :param letter_guessed: letter value
    :type letter_guessed: str
    :return True if the letter is legal, and false otherwise
    :rtype bool """
    length = len(letter_guessed)
    if length > 1:
        return False
    return letter_guessed.isalpha() # will return True if the char is a letter, and False otherwise


def check_valid_input(letter_guessed, old_letters_guessed):
    """checks whether the letter given by the user is legal by using is_valid_input,
    and if it wasn't guessed before.
    :param letter_guessed: the letter given by the user
    :type letter_guessed: str
    :param old_letters_guessed: the previous letters who was guessed by the user
    :type old_letters_guessed: list
    :return True if the input is legal, and False otherwise
    :rtype bool"""
    if is_valid_input(letter_guessed):
        if letter_guessed.lower() in old_letters_guessed:
            return False
        else:
            return True
    return False


def try_update_letter_guessed(letter_guessed, old_letters_guessed):
    """the method updates the guesses list (if the input is legal)
    :param letter_guessed: letter value
    :type letter_guessed: str
    :param old_letters_guessed: the previous letters who was guessed by the user.
    :type old_letters_guessed: list
    :return True if the letter is the letter was updated, and False otherwise
    :rtype bool """
    if check_valid_input(letter_guessed, old_letters_guessed):
        old_letters_guessed.append(letter_guessed.lower())
        return True
    else:
        print("X")
        old_guesses_sorted_list = sorted(old_letters_guessed)
        print(" -> ".join(old_guesses_sorted_list))
        return False


def show_hidden_word(secret_word, old_letters_guessed):
    """this method returns the word the player have guessed so far,
    need to use print() on the returned value.
    :param secret_word: the word the player has to guess
    :type secret_word: str
    :param old_letters_guessed: the letters the user have guessed
    :type old_letters_guessed: list
    :return: a string that represents the word the user's word so far
    :rtype: str"""
    ret = ""
    for letter in secret_word:
        if letter.lower() in old_letters_guessed:  # adds the letter to the str ret
            ret += letter + " "
            continue
        ret += "_ "                                # otherwise, adds "_"
    return ret


def check_win(secret_word, old_letters_guessed):
    """this method checks if the player guessed all the letters in secret_word.
    :param secret_word: the word the player tries to guess
    :type secret_word: str
    :param old_letters_guessed: the letters the player guessed so far
    :type old_letters_guessed: list
    :return: True or False according to the method definition
    :rtype: bool"""
    for letter in secret_word:
        if letter.isalpha():
            if letter.lower() not in old_letters_guessed:
                return False
    return True


def print_hangman(num_of_tries):
    """this method prints the curr state of your hangman"""
    print(HANGMAN_PHOTOS[num_of_tries])


def choose_word(file_path, index):
    """"This method chooses a word out of a file of words,
    :param file_path: the path to the file with the words
    :type file_path: str
    :param index: the index of the chosen word
    :type index: int
    :return: (amount of words, the word chosen)
    :rtype: tuple"""
    file = open(file_path, 'r')
    words = file.read().split()
    amount_of_words = len(set(words))
    chosen_word = words[index % len(words) - 1]
    return amount_of_words,chosen_word


def does_file_exist(file_path):
    """"this method checks whether the file path is legal"""
    return os.path.isfile(file_path)


def is_int(param):
    """"this method checks if the given str can be converted to int"""
    return param.isnumeric()


def main():
    intro_screen()                                    # plays the intro screen for the game
    file_path = input("Enter file path: ")            # asks for a file of words
    while not does_file_exist(file_path):
        file_path = input("Please enter a valid file path: ")
    index = input("Enter index: ")                    # asks for an index
    while not is_int(index):
        index = input("Please enter a valid index: ")
    index = int(index)                                # converts the index to int
    secret_word = choose_word(file_path, index)[1]    # we are using the second element in choose_word's tuple
    print("\nLets start!")
    num_of_tries = MAX_TRIES
    print_hangman(num_of_tries)
    print(show_hidden_word(secret_word, LETTERS_GUESSED))
    while num_of_tries > 0:                           # while we have more tries
        letter_guessed = input("Guess a letter: ").lower()
        if not try_update_letter_guessed(letter_guessed, LETTERS_GUESSED):
            continue
        else:   # try_update_letter_guessed inserts the letter_guessed to LETTERS_GUESSED list
            if letter_guessed not in secret_word:     # the player guessed an incorrect letter
                num_of_tries -= 1
                print(":(")
                print_hangman(num_of_tries)
            print(show_hidden_word(secret_word, LETTERS_GUESSED))
        if check_win(secret_word, LETTERS_GUESSED):
            print("WIN")
            break
    if not check_win(secret_word, LETTERS_GUESSED):     # makes sure we don't print LOSE after the player won
        print("LOSE")


if __name__ == '__main__':
    main()
#

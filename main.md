import random

WORDS = 10
WORDLEN = 40
CHANCE = 6

srand_called = False

def i_rnd(i):
    global srand_called
    if not srand_called:
        random.seed()
        srand_called = True
    return random.randint(0, i - 1)

def decrypt(code):
    hash_val = ((len(code) - 3) // 3) + 2
    decrypt_str = ''
    word = code
    for index, ch in enumerate(code):
        if (index - code.index(word) + 2) % 3 == 1:
            decrypt_str += chr(ord(ch) - (index - code.index(word) + 1) - hash_val)
    return decrypt_str

def print_body(mistakes, body):
    body_chars = [' '] * (CHANCE + 1)
    body_chars[0] = '('
    body_chars[1] = ')'
    body_chars[2] = '/'
    body_chars[3] = '|'
    body_chars[4] = '\\'
    body_chars[5] = '/'
    body_chars[6] = '\\'
    
    body_chars = body_chars[:mistakes+1] + [' '] * (CHANCE - mistakes)
    
    print("\tMistakes :", mistakes)
    print("\t _________")
    print("\t|         |")
    print("\t|        {} {}".format(body_chars[0], body_chars[1]))
    print("\t|        {}{}{}".format(body_chars[2], body_chars[3], body_chars[4]))
    print("\t|        {} {}".format(body_chars[5], body_chars[6]))
    print("\t|")
    print("\t|")

def print_word(guess, len):
    print("\t", end='')
    for i in range(len):
        print(guess[i], end=' ')
    print("\n")

def main():
    print("\n\t Be aware you can be hanged!!.")
    print("\n\n\t Rules : ")
    print("\n\t - Maximum 6 mistakes are allowed.")
    print("\n\t - All alphabet are in lower case.")
    print("\n\t - All words are name of very popular Websites. eg. Google")
    print("\n\t - If you enjoy continue, otherwise close it.")
    print("\n\t Syntax : Alphabet")
    print("\n\t Example : a \n")

    values = ["N~mqOlJ^tZletXodeYgs", "gCnDIfFQe^CdP^^B{hZpeLA^hv", "7urtrtwQv{dt`>^}FaR]i]XUug^GI",
             "aSwfXsxOsWAlXScVQmjAWJG", "cruD=idduvUdr=gmcauCmg]", "BQt`zncypFVjvIaTl]u=_?Aa}F",
             "iLvkKdT`yu~mWj[^gcO|", "jSiLyzJ=vPmnv^`N]^>ViAC^z_", "xo|RqqhO|nNstjmzfiuoiFfhwtdh~",
             "OHkttvxdp|[nnW]Drgaomdq"]

    id = i_rnd(WORDS)
    word = decrypt(values[id])
    len_word = len(word)
    guessed = ['_'] * len_word
    false_word = []

    mistakes = 0

    while mistakes < CHANCE and '_' in guessed:
        print("\n\n")
        print_body(mistakes, false_word)
        print("\n\n")
        print("\tFalse Letters : ", end='')
        if mistakes == 0:
            print("None")
        else:
            print("".join(false_word))
        print("\n\n")
        print_word(guessed, len_word)
        guess = input("\tGive me a alphabet in lower case : ")
        
        found = False
        for i, ch in enumerate(word):
            if ch == guess:
                found = True
                guessed[i] = guess
        if not found:
            false_word.append(guess)
            mistakes += 1

    if '_' not in guessed:
        print("\n")
        print_word(guessed, len_word)
        print("\n\tCongrats! You have won :", word, "\n\n")
    else:
        print("\n")
        print_body(mistakes, false_word)
        print("\n\n\tBetter try next time. Word was", word, "\n\n")


if __name__ == "__main__":
    main()

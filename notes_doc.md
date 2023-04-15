import string

class Notebook:
  """Notebook manages your notes in a local text file
  """
  def __init__(self, filename):
    self.filename = filename
    notesfd = open(filename, 'r')
    self.notes = notesfd.readlines()
    notesfd.close()

  def add_note(self, note):
    """Adds a note to the notebook

    Args:
        note (str): the note you want to add
    """
    self.notes.append(note+'\n')
    self.save()
    print("Note successfully added.")

  def delete_note(self, index):
    """Deletes a note from the notebook

    Args:
        index (int): the number of the note you want to delete
    """
    if int(index)-1 > len(self.notes):
      return False
    
    self.notes.pop(int(index)-1)
    self.save()
    print("Note",index,"has been deleted.")
    return True

  def edit_note(self, note, index):
    """Edits a note in the notebook

    Args:
        note (str): the edited note
        index (int): the number of the note you want to edit
    """
    if index > len(self.notes):
      return False

    self.notes[index-1] = note + '\n'
    self.save()
    print("Note",index,"has been edited.")
    return True
  
  def save(self):
    """Saves a note in the notebook
    """
    notesfd = open(self.filename, 'w')
    notesfd.writelines(self.notes)
    notesfd.close()

  def print_note(self, index):
    """Displays a note from the notebook based on your input

    Args:
        index (int): the number of the note you want to display
    """
    print(index+": "+self.notes[int(index)-1])
  
  def display_notes(self):
    """Displays all notes from the notebook, with each note printed in a new line
    """
    print ("Here are all your notes.")
    for i in range(len(self.notes)):
      print (str(i+1) + ":",self.notes[i])

  def search_notes(self, query):
    """Searches notebook for a string you input

    Args:
        query (str): the string you want to search for
    """
    indices = []
    query = standardize_word(query)
    for i in range(len(self.notes)):
      note = self.notes[i].split()
      for word in note:
        word = standardize_word(word)
        if query in word:
          indices.append(str(i+1))
          if len(indices)>1:
            if indices[-1] == indices[-2]:
              indices.pop()
    if indices == []:
      print("Search query not found.")
    else:
      print ("Found the word",'"'+query+'"', "in notes:",", ".join(indices))

def standardize_word(word):
  """Removes punctuation from your input and makes input all lowercase for the search function
  """
  word = word.strip(string.punctuation)
  word = word.strip()
  word = word.lower()
  return word    
  
def main():
  """Asks you which text file to edit and presents a list of options for editing said file; must include '.txt' for the file when prompted
  
  Args:
    input (str): the file you want to edit
    input (str): the letter or number representing the desired action
  """
  file_opened = False
  while (not file_opened):
    
   try:
      file_input = input("Please enter the text file you would like to edit: ")
      file = Notebook(file_input)
      file_opened = True
   except:
     file_opened = False
     print("File not found. Please try again.")
  
  print("Welcome to Notebook! You are currently editing",file_input+". What would you like to do?")
  print("A. Add a note")
  print("B. Delete a note")
  print("C. Edit a note")
  print("D. Display a note")
  print("E. Search notes")
  print("0. Quit")
  done = False
  while (not done):
    """This tells you if your input is unexpected or can't be handled by the program
    """
    print()
    try:
      choice = input("Please enter the letter next to your selection. ")
      if choice == "A" or choice == "a":
        entry = input("Please enter your new note: ")
        file.add_note(entry)
      elif choice == "B" or choice == "b":
        file.display_notes()
        entry = input("Please enter the number of the note you want to delete: ")
        file.delete_note(entry)
      elif choice == "C" or choice == "c":
        file.display_notes()
        entry_index = input("Please enter number of the note you want to edit: ")
        entry_note = input("Please enter the edited note: ")
        file.edit_note(entry_note,int(entry_index))
      elif choice == "D" or choice == "d":
        entry = input("Please enter the number of the note you want to display. If you want to display all notes, enter 'all'. ")
        if entry == "all":
          file.display_notes()
        else:
          file.print_note(entry)
      elif choice == "E" or choice == "e":
        entry = input("Please enter the word you want to search for: ")
        file.search_notes(entry)
      elif choice == "0":
        done = True
        print("Goodbye!")
      else:
        print("Selection invalid.")
    except:
     print("Selection invalid.")
    
main()

import time
from watchdog.observers import Observer
from watchdog.events import PatternMatchingEventHandler
from skpy import Skype

sk = Skype("user" , "parola") # conrect to Skype


sk.contacts # your contacts



ch = sk.contacts["skype_contact_to_message"].chat # 1-to-1 conversation



if __name__ == "__main__":
    patterns = "*"
    ignore_patterns = ""
    ignore_directories = False
    case_sensitive = True
    my_event_handler = PatternMatchingEventHandler(patterns, ignore_patterns, ignore_directories, case_sensitive)
    def on_created(event):
        ch.sendMsg(f"hey, {event.src_path} has been created!")
        print(f"hey, {event.src_path} has been created!")
    my_event_handler.on_created = on_created

    path = "." # current path where script is
    go_recursively = True
    my_observer = Observer()
    my_observer.schedule(my_event_handler, path, recursive=go_recursively)

    my_observer.start()
    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        my_observer.stop()
        my_observer.join()

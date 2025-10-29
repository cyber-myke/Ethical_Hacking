# Analyzing Network Vulnerabilities with Python

Analyzing network vulnerabilities with Python  

Scenario:  
You have been learning the building blocks of Python programming language and now it is time 
to apply those skills. You will be implementing and using techniques to solve a real-world 
network security related issue, which you may face while working as a Security Professional in 
the field. 
You have been hired by an organization as a security professional and your first task is to 
analyze a network for vulnerabilities. There are many different servers on a network, running 
various services, to support day-to-day business operations. You have been asked to gather 
information about the possible vulnerabilities on any specific server. 

Assignment:  
Your task is to write a Python program that will test a given host and generate a list of all the 
TCP open ports within the range of 1 to 1025. You are required to accomplish this task by using 
the standard Python’s “socket” library. 

 
On execution of the program your code should prompt “Enter a host to scan”. The user will provide a 
host name or IP address. 
1. Your code should look for all of the open TCP ports between the range of 1 to 1025. 
2. If a port is open your code will create a file and add an entry for each port number that is open 
3. In case of any exception for instance “host is not available”, “host name could not be resolved” 
or due to any other error, you need to write the exception into the output file. 
4. You also need to record the starting and ending date and time at the beginning and 
ending of the scan and write that information to the output file. You should also show 
the total time it took to scan the ports.

# Python code for port scanner
import socket
import threading
import time

# Function to perform port scan on a given host and port range
def port_scan(host, port):
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.settimeout(1)
        result = s.connect_ex((host, port))
        if result == 0:
            print("Port {} is open".format(port))
            open_ports.append(port)
        s.close()
    except Exception as e:
        exceptions.append(str(e))

# Function to perform port scan with threading
def threaded_port_scan(host, ports):
    threads = []
    for port in ports:
        t = threading.Thread(target=port_scan, args=(host, port))
        threads.append(t)
        t.start()

    for t in threads:
        t.join()

if __name__ == "__main__":
    host = input("Enter the host or IP address to scan: ")
    start_time = time.time()
    start_date = time.strftime("%Y-%m-%d %H:%M:%S")

    open_ports = []
    exceptions = []

    # Define port range
    ports = range(1, 1027)

    # Perform threaded port scan
    threaded_port_scan(host, ports)

    end_time = time.time()
    end_date = time.strftime("%Y-%m-%d %H:%M:%S")

    total_time = end_time - start_time

    # Print open ports
    print("Open ports:", open_ports)

    # Print start and end time and start and end date
    print("Start Time:", start_date)
    print("End Time:", end_date)

    # Print total time of port scan
    print("Total Time:", total_time)

    # Write results to a text file
    with open("port_scan_results.txt", "w") as file:
        file.write("Start Time: {}\n".format(start_date))
        file.write("End Time: {}\n".format(end_date))
        file.write("Total Time: {} seconds\n\n".format(total_time))

        file.write("Open ports:\n")
        for port in open_ports:
            file.write("{}\n".format(port))

        file.write("\nExceptions:\n")
        for exception in exceptions:
            file.write("{}\n".format(exception))

    print("Results saved to port_scan_results.txt")

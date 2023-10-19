# Part 1: StringServer
my code for `StringServer`, based very heavily off the given `NumberServer` file:
```
class Handler implements URLHandler {
    
    ArrayList<String> strings = new ArrayList<>();

    String words = "";
    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return  words;
        } else {
            if (url.getPath().contains("/add-message")) {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("s")) {
                    strings.add(parameters[1]);
                    words += strings.size() + ". " + parameters[1] + "\n";
                    return words;
                }
            }
            return "404 Not Found!";
        }
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```


# Part 2
private key path:
public key path: 
![successful ssh!](https://github.com/yue-hua-x/cse15l-lab-reports/assets/146787492/61f6c2a0-b7bb-4809-a265-bf24d40f3bc0)
# Part 3
I didn't know there was a way to automatically log in when `ssh`-ing. It 

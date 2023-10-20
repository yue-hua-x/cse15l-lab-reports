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
for the following exercises, i used EdStem because i 1) didn't want to figure out the ucsd vpn thing and 2) ssh... doesn't work on my home pc?? anyways.
## 1. adding 'hello'
![image](https://github.com/yue-hua-x/cse15l-lab-reports/assets/146787492/f83946ac-5ce2-4b74-a707-de8c81626c23)\
the method called in my code was the only method that isn't the `main` method: the `handleRequest` method in `URLHandler`. the argument to that method was the url that the browser passed to my server, and the only field of the class, an `ArrayList` called `strings` started out empty before the request. after this request, I added a string containing the message `hello` to `strings`.

## 2. adding 'how are you'
![image](https://github.com/yue-hua-x/cse15l-lab-reports/assets/146787492/b97aa651-b8f2-4a93-8fd8-eae0b341c071)\
so, unexpected result. maybe a result of using firefox? who knows. `handleRequest` was called again to handle the request, and the contents of `strings` up to this point is just `{"hello"}`. probably due to how my browser processes URLs, the server didn't actually receive "how are you", but rather "how+are+you" as the message part of the query. therefore, the content of `strings` after the request would be `{"hello", "how+are+you"}`.

## 3. adding 'how are you' (part 2, electric boogaloo)
![image](https://github.com/yue-hua-x/cse15l-lab-reports/assets/146787492/3f5f0173-08e8-4931-9d56-70d39b78aba8)\
after a bit of cursory googling, i went and replaced all of my spaces with `%C2%A0`. not sure why it works, but it does?

# Part 2
private key path: `/home/yuehua/.ssh/id_rsa.pub`\
public key path: `/home/linux/ieng6/cs15lfa23/cs15lfa23hg/.ssh/authorized_keys`\
![successful ssh!](https://github.com/yue-hua-x/cse15l-lab-reports/assets/146787492/61f6c2a0-b7bb-4809-a265-bf24d40f3bc0)
# Part 3
I didn't know there was a way to automatically log in when `ssh`-ing. It makes sense, since constantly sending passwords back and forth doesn't sound like good security, and on an intellectual level I guess I would've been more surprised to learn that there *wasn't* a way to automatically log in, but I just didn't know or bother to find out.

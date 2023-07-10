import os, requests, random, threading, json, time, multiprocessing
from colorama import Fore

# Credit to Pycenter by billythegoat356
# Github: https://github.com/billythegoat356/pycenter/
# License: https://github.com/billythegoat356/pycenter/blob/main/LICENSE

def center(var:str, space:int=None): # From Pycenter
    if not space:
        space = (os.get_terminal_size().columns - len(var.splitlines()[int(len(var.splitlines())/2)])) / 2
    
    return "\n".join((' ' * int(space)) + var for var in var.splitlines())

class Console():        
    def ui(self):
        os.system(f'cls && title [DNG] Discord Nitro Generator  ^|  For Help join discord.gg/kaneki' if os.name == "nt" else "clear")
        print(center(f"""\n\n
██████╗ ███╗   ██╗ ██████╗ 
██╔══██╗████╗  ██║██╔════╝            ~ Discord Nitro Generator ~
██║  ██║██╔██╗ ██║██║  ███╗     
██║  ██║██║╚██╗██║██║   ██║     github.com/kanekiWeb ~ skulldev.ga
██████╔╝██║ ╚████║╚██████╔╝ 
╚═════╝ ╚═╝  ╚═══╝ ╚═════╝ \n\n
              """).replace('█', Fore.CYAN+"█"+Fore.RESET).replace('~', Fore.CYAN+"~"+Fore.RESET).replace('-', Fore.CYAN+"-"+Fore.RESET))

    def printer(self, color, status, code):
        threading.Lock().acquire()
        print(f"{color} {status} > {Fore.RESET}discord.gift/{code}")
    
    def proxies_count(self):
        proxies_list = 0
        with open('config/proxies.txt', 'r') as file:
            proxies = [line.strip() for line in file]
        
        for _ in proxies:
            proxies_list += 1
        
        return int(proxies_list)


class Worker():              
    def random_proxy(self):
        with open('config/proxies.txt', 'r') as f:
            proxies = [line.strip() for line in f]
        
        return random.choice(proxies)

    def config(self, args, args2=False):
        with open('config/config.json', 'r') as conf:
            data = json.load(conf)
        
        if args2:
            return data[args][args2]
        else:
            return data[args]
    
    def run(self):
        self.code = "".join(random.choice("abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890") for _ in range(16))
        try:
            req = requests.get(f'https://discordapp.com/api/v6/entitlements/gift-codes/{self.code}?with_application=false&with_subscription_plan=true', proxies={'http': self.config("proxies")+'://'+self.random_proxy(),'https': self.config("proxies")+'://'+self.random_proxy()}, timeout=1)
            
            if req.status_code == 200:
                Console().printer(Fore.LIGHTGREEN_EX, " Valid ", self.code)
                open('results/hit.txt', 'a+').write(self.code+"\n")
                try:
                    requests.post(Worker().config("webhook", "url"), json={"content": f"||@here|| **__New Valid Nitro !!__**\n\nhttps://discord.gift/{self.code}", "username": Worker().config("webhook", "username"), "avatar_url": Worker().config("webhook", "avatar")})
                except:
                    pass
            elif req.status_code == 404:
                Console().printer(Fore.LIGHTRED_EX, "Invalid", self.code)
            elif req.status_code == 429:
                # rate = (int(req.json()['retry_after']) / 1000) + 1
                Console().printer(Fore.LIGHTBLUE_EX, "RTlimit", self.code)
                # time.sleep(rate)
            else:
                Console().printer(Fore.LIGHTYELLOW_EX, " Retry ", self.code)
                  
        except KeyboardInterrupt:
            Console().ui()
            threading.Lock().acquire()
            print(f"{Fore.LIGHTRED_EX} Stopped > {Fore.RESET}Nitro Gen Stopped by Keyboard Interrupt.")
            os.system('pause >nul')
            exit()
        except:
            # Console().printer(Fore.LIGHTRED_EX, "Invalid", self.code)
            Console().printer(Fore.LIGHTYELLOW_EX, " Retry ", self.code)
        
if __name__ == "__main__":
    Console().ui()
    print(" "+Fore.CYAN + str(Console().proxies_count()) + Fore.RESET + " Total proxies loaded...\n\n")
    DNG = Worker()
    
    while True:
        if threading.active_count() <= int(Worker().config("thread")):  
            threading.Thread(target=DNG.run(), args=()).start()

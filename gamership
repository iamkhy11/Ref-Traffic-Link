import threading
import time
import random
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By


TARGET_URL = "https://gamership.org/go/new/"
KEYWORD = "gacor300"
NUM_THREADS = 3
WAVE_DELAY = 10
TOTAL_STAY_TIME = 60 


PROXY_LIST = [
    
]

def simulate_human_visit(thread_id):
    options = Options()
    options.add_argument("--headless")
    options.add_argument("--no-sandbox")
    options.add_argument("--disable-dev-shm-usage")
    options.add_argument("--window-size=1366,768")
    options.add_argument("--disable-blink-features=AutomationControlled")
    options.add_experimental_option("excludeSwitches", ["enable-automation", "enable-logging"])
    options.add_experimental_option('useAutomationExtension', False)
    
    
    if PROXY_LIST:
        proxy = random.choice(PROXY_LIST)
        options.add_argument(f'--proxy-server={proxy}')
        print(f"[Thread {thread_id}] Menggunakan Proxy: {proxy}")

    options.add_argument(f"user-agent={random_human_ua()}")

    driver = None
    try:
        driver = webdriver.Chrome(options=options)
        driver.execute_script("Object.defineProperty(navigator, 'webdriver', {get: () => undefined})")
        driver.set_page_load_timeout(45) 
        
        
        print(f"[Thread {thread_id}] Mencari di Google...")
        driver.get(f"https://www.google.co.id/search?q={KEYWORD.replace(' ', '+')}")
        time.sleep(random.randint(5, 8))
        
        
        print(f"[Thread {thread_id}] Membuka Target Utama...")
        driver.get(TARGET_URL)
        
        
        human_scroll(driver, thread_id, 30)

        
        print(f"[Thread {thread_id}] Mencari produk untuk diklik...")
        try:
            links = driver.find_elements(By.CSS_SELECTOR, "a[href*='/product/'], a[href*='/shop/']")
            if links:
                target_link = random.choice(links[:10])
                href = target_link.get_attribute('href')
                print(f"[Thread {thread_id}] Klik ke produk: {href}")
                driver.execute_script("arguments[0].click();", target_link)
                
                
                human_scroll(driver, thread_id, 30)
            else:
                human_scroll(driver, thread_id, 30)
        except:
            human_scroll(driver, thread_id, 30)

        print(f"[Thread {thread_id}] Berhasil! Total sesi 60s+ tercapai.")

    except Exception as e:
        print(f"[Thread {thread_id}] Gagal: {str(e)[:50]}")
    finally:
        if driver:
            driver.quit()

def human_scroll(driver, thread_id, duration):
    start_time = time.time()
    while time.time() - start_time < duration:
        amount = random.randint(150, 450)
        driver.execute_script(f"window.scrollBy(0, {amount});")
        time.sleep(random.randint(5, 9))

def random_human_ua():
    uas = [
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.0.0 Safari/537.36",
        "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"
    ]
    return random.choice(uas)

def start_wave():
    wave = 1
    while True:
        print(f"\n--- GELOMBANG {wave} ---")
        threads = []
        for i in range(NUM_THREADS):
            t = threading.Thread(target=simulate_human_visit, args=(i+1,))
            t.start()
            threads.append(t)
            time.sleep(random.randint(3, 8))
        
        for t in threads:
            t.join()
        
        print(f"\nGelombang {wave} Selesai. Istirahat {WAVE_DELAY}s...")
        time.sleep(WAVE_DELAY)
        wave += 1

if __name__ == "__main__":
    start_wave()

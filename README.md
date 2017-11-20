# pygal + BeautifulSoup + PyQt5

import sys
import bs4, requests, json, urllib.request
from bs4 import BeautifulSoup
from PyQt5.QtWebEngineWidgets import QWebEnginePage
from PyQt5.QtWidgets import QApplication
from PyQt5.QtCore import QUrl
import pygal
from pygal.style import Style


class Page(QWebEnginePage):
    def __init__(self, url):
        self.app = QApplication(sys.argv)
        QWebEnginePage.__init__(self)
        self.html = ''
        self.loadFinished.connect(self._on_load_finished)
        self.load(QUrl(url))
        self.app.exec_()

    def _on_load_finished(self):
        self.html = self.toHtml(self.Callable)
        print('Load finished')

    def Callable(self, html_str):
        self.html = html_str
        self.app.quit()
        
def main():
    page = Page("http://destinytracker.com/d2/profile/xbl/dijiphos")
    soup = BeautifulSoup(page.html, 'html.parser')
    PvP = soup.find('div', class_='gt-pvp-casual')
    stats = PvP.find('div', class_='trn-stats')
    diji_value = stats.find('span', class_='value')

    page = Page("http://destinytracker.com/d2/profile/xbl/punisherdonk")
    soup = BeautifulSoup(page.html, 'html.parser')
    PvP = soup.find('div', class_='gt-pvp-casual')
    stats = PvP.find('div', class_='trn-stats')
    donk_value = stats.find('span', class_='value')

    page = Page("http://destinytracker.com/d2/profile/xbl/musclemuffins20")
    soup = BeautifulSoup(page.html, 'html.parser')
    PvP = soup.find('div', class_='gt-pvp-casual')
    stats = PvP.find('div', class_='trn-stats')
    musc_value = stats.find('span', class_='value')

    diji = diji_value.text
    donk = donk_value.text
    musc = musc_value.text

    bar_chart = pygal.Bar()
    bar_chart.title = "Destiny KD"
    bar_chart.add("Dijiphos", float(diji))
    bar_chart.add("Punisher Donk", float(donk))
    bar_chart.add("musclemuffins20", float(musc))
    bar_chart.render_in_browser()

if __name__ == '__main__': main()

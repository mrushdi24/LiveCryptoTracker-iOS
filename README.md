# Live Crypto Tracker - iOS App

An iOS application that tracks live cryptocurrency prices using Binance WebSocket API. The app supports real-time updates, Core Data storage for favorite coins, and temporary caching via UserDefaults.

Features

- Live cryptocurrency ticker via WebSocket
- Favorite coins stored with Core Data
- Dynamic connection based on saved favorites
- Add/remove coins through a separate UI
- Local cache using UserDefaults for performance
- MVVM architecture (UIKit-based)

Project Structure

CryptoTrackerApp/
├── Models/
│   └── Crypto.swift
│   └── CryptoStored+CoreData.swift
├── ViewModels/
│   └── PriceTickerViewModel.swift
├── Views/
│   └── HomeVC.swift
│   └── CryptoFavouritVC.swift
│   └── CryptoIColeectionCell.swift
├── Services/
│   └── StoreManager.swift
│   └── CryptoManager.swift
│   └── CryptoCacheManager.swift
├── Extensions/
│   └── Notification+Names.swift
└── Resources/
    └── Assets.xcassets
    └── LaunchScreen.storyboard

Architecture

The app follows a clean MVVM structure:

- Model: Crypto, CryptoStored (Core Data), CryptoDataModel
- ViewModel: PriceTickerViewModel (handles WebSocket logic)
- ViewController: HomeVC, CryptoFavouritVC (UI + delegation)
- Protocol: RefreshCryptoProtocol for view communication

WebSocket Connection

- Binance stream: wss://stream.binance.com:9443/stream?streams=SYMBOL@ticker
- Symbols are dynamically generated from Core Data favorites.
- Reconnects automatically when the favorite list changes.

Core Data Storage

StoreManager.swift handles:

- Saving/removing favorite coins
- Fetching stored coins for WebSocket streaming

func fetchStore(completion: @escaping ([CryptoStored]) -> Void)
func saveToStore(for crypto: CryptoDataModel, completion: (Bool, Bool) -> Void)
func deleteSymbol(_ symbol: String, completion: (() -> Void)?)

Caching System (UserDefaults)

A lightweight cache layer stores coin metadata before committing to Core Data.

CryptoCacheManager.swift provides:

func saveToCache(_ cryptos: [CryptoDataModel])
func loadFromCache() -> [CryptoDataModel]
func addToCache(_ crypto: CryptoDataModel)
func removeFromCache(_ crypto: CryptoDataModel)
func isInCache(_ name: String) -> Bool

Benefits:

- Prevents duplicate saving
- Boosts performance for coin info
- Useful for offline fallback

View Communication

Uses a custom delegate protocol:

protocol RefreshCryptoProtocol {
    func refrechCrypto()
}

- Called when the user adds/removes a favorite coin.
- Triggers viewModel.updateConnection() to reconnect WebSocket.

Future Improvements

- SwiftUI version with @ObservableObject
- Combine-based version instead of manual delegation
- Local search & filter
- User authentication + remote sync
- Charts and trends per coin

Requirements

- iOS 14+
- Xcode 14+
- Internet connection to fetch Binance WebSocket data

Dependencies

- UIKit
- CoreData
- URLSession WebSocket
- Combine (for @Published)

Screenshots

<p float="left">
  <img src="https://fotonicapp.com/travel/Api/crypto/splash-screen.png" width="250" />
  <img src="https://fotonicapp.com/travel/Api/crypto/home-screen.png" width="250" />
  <img src="https://fotonicapp.com/travel/Api/crypto/crypto-screen.png" width="250" />
</p>




License

This project is licensed under the MIT License.

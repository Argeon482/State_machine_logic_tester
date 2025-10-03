# State Machine Logic Tester

A web-based interactive tool for testing and debugging state machine logic, specifically designed for game bot automation systems.

## 🚀 Live Demo

Visit the live application: [State Machine Logic Tester](https://your-username.github.io/your-repo-name)

## 📋 Features

- **Interactive State Machine Simulator**: Test complex bot logic with real-time state transitions
- **Trigger Management**: Toggle up to 20 configurable triggers with visual indicators
- **Scenario Testing**: Pre-built scenarios to validate specific logic paths
- **Real-time Logging**: Detailed console output with timestamped events
- **Configuration Management**: 
  - Customizable logic profiles (JSON-based)
  - Scenario scripting system
  - Persistent settings via localStorage
- **Manual & Automated Testing**: 
  - Single-step execution
  - Continuous cycling with configurable delays
  - Scenario-driven automated testing

## 🛠️ Usage

### Quick Start
1. Open the application in your browser
2. Use the default "Original GameBot Logic" profile to explore
3. Toggle triggers by clicking on them in the main interface
4. Run single cycles or load scenarios for automated testing

### Configuration
- Click the ⚙️ settings button to access configuration
- **Logic Profile Tab**: Define states, triggers, and transition logic
- **Scenarios Tab**: Create automated test scenarios
- **Display Triggers Tab**: Select which triggers to show on the main interface

### Testing Scenarios
The application includes several pre-built scenarios:
1. **Full Grinding Loop**: Tests complete combat and inventory management cycle
2. **Goal Save & Resupply Retreat**: Validates proactive resource management
3. **Market Crisis & S_EXIT**: Tests failure recovery and termination conditions

## 🔧 Development

### Local Development
Simply open `index.html` in any modern web browser. No build process required.

### GitHub Pages Deployment
This repository is configured for automatic GitHub Pages deployment:

1. **Enable GitHub Pages**:
   - Go to your repository settings
   - Navigate to "Pages" section
   - Under "Source", select "GitHub Actions"

2. **Automatic Deployment**:
   - Push changes to `main` or `master` branch
   - GitHub Actions will automatically build and deploy
   - Your site will be available at `https://your-username.github.io/your-repo-name`

### File Structure
```
├── index.html              # Main application file
├── .github/
│   └── workflows/
│       └── deploy.yml      # GitHub Pages deployment workflow
└── README.md              # This file
```

## 🎯 Use Cases

- **Bot Logic Validation**: Test state machine logic before deployment
- **Debugging**: Identify edge cases and logic flaws
- **Documentation**: Demonstrate bot behavior to stakeholders
- **Training**: Learn state machine concepts interactively

## 🔒 Privacy

All data is stored locally in your browser's localStorage. No data is transmitted to external servers.

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

---

**Note**: Replace `your-username` and `your-repo-name` in the URLs above with your actual GitHub username and repository name.
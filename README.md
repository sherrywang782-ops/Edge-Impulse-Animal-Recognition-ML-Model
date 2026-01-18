# Edge Impulse Animal Recognition

An Edge Impulse project for building a machine learning model that can recognize different animals using computer vision. This repository contains the deployment pipeline and generated model artifacts for edge deployment.

## Overview

This project uses Edge Impulse to train a neural network model capable of classifying images of various toy animals. The trained model is automatically built and deployed using GitHub Actions, generating optimized C++ libraries and TensorFlow Lite models for edge devices.

## Features

- **Automated CI/CD Pipeline**: GitHub Actions workflow that automatically builds and deploys the latest model version
- **Multi-format Deployment**: Generates both C++ libraries (with Edge Impulse SDK) and TensorFlow Lite models
- **Version Tracking**: Automatic versioning and commit tracking of model updates
- **Edge-Optimized**: Models are optimized for deployment on resource-constrained edge devices

## Project Structure

```
├── .github/workflows/          # GitHub Actions CI/CD pipeline
│   └── build_model.yml        # Workflow for building Edge Impulse model
├── libs/                      # Generated Edge Impulse SDK and libraries
├── src/                       # Model parameters and configuration
├── models/                    # TensorFlow Lite model files
├── version.txt                # Build information and version tracking
└── README.md                  # This file
```

## Prerequisites

Before using this project, you'll need:

1. **Edge Impulse Account**: Sign up at [edgeimpulse.com](https://edgeimpulse.com)
2. **Edge Impulse Project**: Create a project and train your animal recognition model
3. **GitHub Repository Secrets**:
   - `PROJECT_ID`: Your Edge Impulse project ID
   - `API_KEY`: Your Edge Impulse API key

## Setup Instructions

### 1. Create Edge Impulse Project

1. Go to [Edge Impulse Studio](https://studio.edgeimpulse.com)
2. Create a new project called "Animal Recognition"
3. Upload training data with images of different animals
4. Design your impulse (image preprocessing + neural network)
5. Train and test your model
6. Deploy your model (this will be done automatically via CI/CD)

### 2. Get Project Credentials

1. In your Edge Impulse project, go to **Dashboard** → **Keys**
2. Copy the **Project ID** and **API Key**

### 3. Configure GitHub Secrets

1. Go to your GitHub repository
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Add the following secrets:
   - `PROJECT_ID`: Paste your Edge Impulse project ID
   - `API_KEY`: Paste your Edge Impulse API key

### 4. Trigger Build

The workflow will automatically run when you:

- Push to the `main` branch
- Create a pull request to `main`
- Manually trigger via GitHub Actions

## Usage

### For Embedded Development (C++)

The generated `libs/` directory contains the Edge Impulse SDK and your trained model. Use this for deploying to microcontrollers or embedded systems:

```cpp
#include "edge-impulse-sdk/classifier/ei_run_classifier.h"

// Your image data (RGB888 format)
ei_impulse_result_t result = { 0 };

signal_t signal;
numpy::signal_from_buffer(your_image_buffer, your_image_size, &signal);

EI_IMPULSE_ERROR res = run_classifier(&signal, &result, false);

if (res == EI_IMPULSE_OK) {
    // Process results
    for (size_t i = 0; i < EI_CLASSIFIER_LABEL_COUNT; i++) {
        printf("%s: %.2f\n", ei_classifier_inferencing_categories[i],
               result.classification[i].value);
    }
}
```

### For Mobile/Web Deployment (TensorFlow Lite)

The `models/` directory contains optimized TensorFlow Lite models that can be used in mobile apps, web applications, or other platforms supporting TFLite:

```python
import tensorflow as tf

# Load the model
interpreter = tf.lite.Interpreter(model_path="models/latest.tflite")
interpreter.allocate_tensors()

# Prepare input
input_details = interpreter.get_input_details()
output_details = interpreter.get_output_details()

# Run inference
interpreter.set_tensor(input_details[0]['index'], input_data)
interpreter.invoke()
output_data = interpreter.get_tensor(output_details[0]['index'])
```

## Model Performance

_Note: Performance metrics will be available after your first successful build. Check the `version.txt` file for build information._

## Development

### Local Testing

To test the workflow locally (requires Docker):

```bash
# Clone the repository
git clone https://github.com/yourusername/Edge-Impulse-Animal-Recognition.git
cd Edge-Impulse-Animal-Recognition

# Set up secrets (for local testing)
export PROJECT_ID="your_project_id"
export API_KEY="your_api_key"

# Run the build script manually (if you create one)
# Or trigger GitHub Actions manually
```

### Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test locally
5. Submit a pull request

## Troubleshooting

### Common Issues

**Workflow fails during "Validate secrets" step**

- Ensure both `PROJECT_ID` and `API_KEY` secrets are set in your GitHub repository
- Check that the secrets are named exactly `PROJECT_ID` and `API_KEY` (case-sensitive)
- Verify the secrets have the correct values from your Edge Impulse project

**Workflow fails with "Invalid API key"**

- Verify your `API_KEY` secret is correctly set in GitHub
- Check that your Edge Impulse API key hasn't expired

**Workflow fails with "Project not found"**

- Verify your `PROJECT_ID` matches exactly with your Edge Impulse project
- Ensure the project exists and is accessible

**No model files generated**

- Check that your Edge Impulse project has a trained impulse
- Verify the deployment settings in Edge Impulse Studio

**Build timeout**

- Large models may take longer to build; consider increasing timeout in workflow

### Getting Help

- [Edge Impulse Documentation](https://docs.edgeimpulse.com)
- [Edge Impulse Community Forum](https://forum.edgeimpulse.com)
- [GitHub Issues](https://github.com/yourusername/Edge-Impulse-Animal-Recognition/issues)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [Edge Impulse](https://edgeimpulse.com) for providing the machine learning platform
- [TensorFlow](https://tensorflow.org) for the machine learning framework
- [GitHub Actions](https://github.com/features/actions) for CI/CD automation

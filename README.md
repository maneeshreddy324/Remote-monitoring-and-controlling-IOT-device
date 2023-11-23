# Remote-monitoring-and-controlling-IOT-device
from flask import Flask, request, jsonify

app = Flask(__name__)

# Dummy database to store device states
device_states = {}

# Endpoint to get the state of a device
@app.route('/api/device/<device_id>', methods=['GET'])
def get_device_state(device_id):
    if device_id in device_states:
        return jsonify({'device_id': device_id, 'state': device_states[device_id]})
    else:
        return jsonify({'error': 'Device not found'}), 404

# Endpoint to update the state of a device
@app.route('/api/device/<device_id>', methods=['PUT'])
def update_device_state(device_id):
    if device_id in device_states:
        data = request.get_json()
        new_state = data.get('state')
        device_states[device_id] = new_state
        return jsonify({'device_id': device_id, 'state': new_state})
    else:
        return jsonify({'error': 'Device not found'}), 404

if __name__ == '__main__':
    app.run(debug=True)

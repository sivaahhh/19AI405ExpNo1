<h1>ExpNo 1 :Developing AI Agent with PEAS Description</h1>
<h3>Name:SIVA SAKTHI.A</h3>
<h3>Register Number:25018439</h3>
AIM:

To find the PEAS description for the given AI problem and develop an AI agent.


Theory
Medicine prescribing agent:
Such this agent prescribes medicine for fever (greater than 98.5 degrees) which we consider here as unhealthy, by the user temperature input, and another environment is rooms in the hospital (two rooms). This agent has to consider two factors one is room location and an unhealthy patient in a random room, the agent has to move from one room to another to check and treat the unhealthy person. The performance of the agent is calculated by incrementing performance and each time after treating in one room again it has to check another room so that the movement causes the agent to reduce its performance. Hence, agents prescribe medicine to unhealthy.

PEAS DESCRIPTION:
Agent Type	Performance	Environment	Actuators	Sensors
Medicine prescribing agent	Treating unhealthy, agent movement	Rooms, Patient	Medicine, Treatment	Location, Temperature of patient
DESIGN STEPS
STEP 1:Identifying the input:
Temperature from patients, Location.

STEP 2:Identifying the output:
Prescribe medicine if the patient in a random has a fever.

STEP 3:Developing the PEAS description:
PEAS description is developed by the performance, environment, actuators, and sensors in an agent.

STEP 4:Implementing the AI agent:
Treat unhealthy patients in each room. And check for the unhealthy patients in random room

STEP 5:
Measure the performance parameters: For each treatment performance incremented, for each movement performance decremented

import random
import time

class Thing:
    """Represents any physical object that can appear in an Environment."""
    
    def is_alive(self):
        return hasattr(self, "alive") and self.alive

    def show_state(self):
        print("I don't know how to show_state.")

class Agent(Thing):
    """An Agent is a subclass of Thing."""
    
    def __init__(self, program=None):
        self.alive = True
        self.performance = 0
        self.program = program

    def can_grab(self, thing):
        return False

def TableDrivenAgentProgram(table):
    percepts = []
    
    def program(percept):
        percepts.append(percept)
        return table.get(tuple(percepts), "No Action")
    
    return program

room_A, room_B = (0,0), (1,0)

def TableDrivenDoctorAgent():
    """Tabular approach towards hospital function."""
    table = {
        ((room_A, "healthy"),): "Right",
        ((room_A, "unhealthy"),): "treat",
        ((room_B, "healthy"),): "Left",
        ((room_B, "unhealthy"),): "treat",
        ((room_A, "unhealthy"), (room_A, "healthy")): "Right",
        ((room_A, "healthy"), (room_B, "unhealthy")): "treat",
        ((room_B, "healthy"), (room_A, "unhealthy")): "treat",
        ((room_B, "unhealthy"), (room_B, "healthy")): "Left",
        ((room_A, "unhealthy"), (room_A, "healthy"), (room_B, "unhealthy")): "treat",
        ((room_B, "unhealthy"), (room_B, "healthy"), (room_A, "unhealthy")): "treat",
    }
    return Agent(TableDrivenAgentProgram(table))

class Environment:
    """Abstract class representing an Environment."""
    
    def __init__(self):
        self.things = []
        self.agents = []

    def percept(self, agent):
        raise NotImplementedError

    def execute_action(self, agent, action):
        raise NotImplementedError

    def is_done(self):
        return not any(agent.is_alive() for agent in self.agents)

    def step(self):
        if not self.is_done():
            actions = [agent.program(self.percept(agent)) for agent in self.agents if agent.alive]
            for agent, action in zip(self.agents, actions):
                self.execute_action(agent, action)

    def run(self, steps=1000):
        for _ in range(steps):
            if self.is_done():
                return
            self.step()

    def add_thing(self, thing, location=None):
        if not isinstance(thing, Thing):
            thing = Agent(thing)
        if thing in self.things:
            print("Can't add the same thing twice")
        else:
            thing.location = location if location else self.default_location(thing)
            self.things.append(thing)
            if isinstance(thing, Agent):
                self.agents.append(thing)

    def default_location(self, thing):
        return None

class TrivialDoctorEnvironment(Environment):
    """Environment with two rooms where patients can be healthy or unhealthy."""
    
    def __init__(self):
        super().__init__()
        self.status = {room_A: random.choice(["healthy", "unhealthy"]), room_B: random.choice(["healthy", "unhealthy"])}

    def percept(self, agent):
        return agent.location, self.status[agent.location]

    def execute_action(self, agent, action):
        if action == "Right":
            agent.location = room_B
            agent.performance -= 1
        elif action == "Left":
            agent.location = room_A
            agent.performance -= 1
        elif action == "treat":
            try:
                temp = float(input("Enter patient's temperature: "))
                if temp >= 98.5:
                    print("Medicine prescribed: Paracetamol and low-dose antibiotic.")
                self.status[agent.location] = "healthy"
                agent.performance += 10
            except ValueError:
                print("Invalid input. Please enter a valid temperature.")

    def default_location(self, thing):
        return random.choice([room_A, room_B])

if __name__ == "__main__":
    agent = TableDrivenDoctorAgent()
    environment = TrivialDoctorEnvironment()
    environment.add_thing(agent)
    
    print("\nStatus of patients in rooms before treatment:")
    print(environment.status)
    print(f"Agent Location: {agent.location}")
    print(f"Performance: {agent.performance}")
    time.sleep(2)

    for _ in range(2):
        environment.run(steps=1)
        print("\nStatus of patients in rooms after treatment:")
        print(environment.status)
        print(f"Agent Location: {agent.location}")
        print(f"Performance: {agent.performance}")
        time.sleep(2)


OUTPUT

image <img width="497" height="344" alt="image" src="https://github.com/user-attachments/assets/ebe20bab-6654-46bc-ae4d-6f4e936d55fb" />


RESULT:
Thus the Developing AI Agent with PEAS Description was implemented using python programming

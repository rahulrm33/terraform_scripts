# Configure the Google Cloud provider
provider "google" {
  credentials = file("tf_demo_auth.json")
  project = "platform-dev-base"
  region  = "us-central1"
  zone    = "us-central1-c"
}

# Create a VPC network and a subnet
resource "google_compute_network" "vpc_network" {
  name                    = "my-vpc-network"
  auto_create_subnetworks = false
}

resource "google_compute_subnetwork" "private_subnet" {
  name          = "private-subnet"
  network       = google_compute_network.vpc_network.self_link
  ip_cidr_range = "10.0.1.0/24"
}

# Create a GKE cluster in the private subnet
resource "google_container_cluster" "cluster" {
  name               = "my-cluster"
  location           = "us-central1-f"
  initial_node_count = 1

  network = google_compute_network.vpc_network.self_link

  subnetwork = google_compute_subnetwork.private_subnet.self_link

}

# Create a firewall rule to restrict inbound traffic
resource "google_compute_firewall" "allow_http" {
  name    = "allow-http"
  network = google_compute_network.vpc_network.self_link

  allow {
    protocol = "tcp"
    ports    = ["80","22","443"]
  }

  source_ranges = ["0.0.0.0/0"]
}

# Create a firewall rule to restrict outbound traffic
resource "google_compute_firewall" "allow_dns" {
  name    = "allow-dns"
  network = google_compute_network.vpc_network.self_link

  allow {
    protocol = "udp"
    ports    = ["80","22","443"]
  }

  source_ranges = ["0.0.0.0/0"]
}






#resource "random_id" "instance_id" {
#  byte_length = 8
#}
#
#resource "google_compute_instance" "default" {
#  name         = "flask-vm-${random_id.instance_id.hex}"
#  machine_type = "f1-micro"
#  zone         = "us-west1-a"
#
#  boot_disk {
#    initialize_params {
#      image = "debian-cloud/debian-11"
#    }
#  }
#
#  metadata_startup_script = "sudo apt-get update; sudo apt-get install -yq build-essential python3-pip rsync; pip install flask"
#
#  network_interface { 
#    network = "default"
#    
#    access_config {
#    }
#  }
#}

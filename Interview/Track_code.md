# Track Service Implementation Walkthrough

This document provides a detailed explanation of the provided code and suggestions for improving and preparing for interview questions based on the project.

## Project Overview
The provided code implements a music track management service using Spring Boot. The key functionalities include:

1. **Creating a Track**: Adding new music tracks to the database.
2. **Fetching All Tracks**: Retrieving a list of all tracks.
3. **Deleting a Track**: Removing a track by its ID.
4. **Sorting Tracks**: Returning a list of tracks sorted by a specific attribute (e.g., play count).

The project uses:
- Spring Boot for application setup and REST API creation.
- Spring Data JPA for database interaction.
- H2 or any other relational database.

---

## Code Walkthrough

### `Track` Entity
The `Track` entity represents the track table in the database.

```java
package com.music.track.model;

import jakarta.persistence.*;
import java.util.Date;

@Entity
@Table(name = "track")
public class Track {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "track_id")
    private Long id;

    @Column(name = "title")
    private String title;

    @Column(name = "album_name")
    private String albumName;

    @Column(name = "release_date")
    private Date releaseDate;

    @Column(name = "playCount")
    private Integer playCount;

    // Getters and Setters
}
```

- **Annotations**:
  - `@Entity`: Marks the class as a JPA entity.
  - `@Table`: Maps the entity to the `track` table.
  - `@Id` and `@GeneratedValue`: Define the primary key and its auto-generation strategy.

- **Fields**:
  - `id`: Unique identifier for each track.
  - `title`: Name of the track.
  - `albumName`: Album to which the track belongs.
  - `releaseDate`: Release date of the track.
  - `playCount`: Number of times the track has been played.

---

### `TrackRequest` DTO

```java
package com.music.track.dto;

import java.util.Date;

public record TrackRequest(String title,
                           String albumName,
                           Date releaseDate,
                           Integer playCount) {
}
```

- The `TrackRequest` DTO simplifies input validation and reduces coupling between the entity and controller.
- It ensures only necessary fields are passed from the client.

---

### `TrackController`

```java
package com.music.track.controller;

import com.music.track.dto.TrackRequest;
import com.music.track.model.Track;
import com.music.track.service.TrackService;
import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.*;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("music/platform/v1/tracks")
public class TrackController {

    private final TrackService trackService;

    @Autowired
    public TrackController(TrackService trackService) {
        this.trackService = trackService;
    }

    @PostMapping()
    public ResponseEntity<Track> createTrack(@RequestBody TrackRequest trackRequest) {
        try {
            Track created = trackService.createTrack(trackRequest);
            return ResponseEntity.status(HttpStatus.CREATED).body(created);
        } catch (Exception e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(null);
        }
    }

    @GetMapping()
    public ResponseEntity<List<Track>> getAllTracks() {
        return null;
    }

    @DeleteMapping("/{trackId}")
    public ResponseEntity<Void> deleteTrack(@PathVariable Long trackId) {
        return null;
    }

    @GetMapping("/sorted")
    public ResponseEntity<List<Track>> getTracksSorted() {
        return null;
    }
}
```

- **Endpoints**:
  - `POST /tracks`: Adds a new track.
  - `GET /tracks`: Fetches all tracks.
  - `DELETE /tracks/{trackId}`: Deletes a track by ID.
  - `GET /tracks/sorted`: Retrieves tracks sorted by an attribute.

---

### `TrackService` Interface

```java
package com.music.track.service;

import com.music.track.dto.TrackRequest;
import com.music.track.model.Track;
import java.util.List;

public interface TrackService {
    Track createTrack(TrackRequest trackRequest);
    List<Track> getAllTracks();
    void deleteTrack(Long trackId);
    List<Track> sortedTracks();
}
```

- Defines the contract for the service layer.
- Implements CRUD operations and sorting logic.

---

### `TrackServiceImpl`

```java
package com.music.track.service.impl;

import com.music.track.dto.TrackRequest;
import com.music.track.model.Track;
import com.music.track.service.TrackService;
import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.music.track.repository.TrackRepository;

@Service
public class TrackServiceImpl implements TrackService {

    @Autowired
    private TrackRepository trackRepository;

    public Track createTrack(TrackRequest trackRequest) {
        Track track = new Track();
        track.setTitle(trackRequest.title());
        track.setAlbumName(trackRequest.albumName());
        track.setReleaseDate(trackRequest.releaseDate());
        track.setPlayCount(trackRequest.playCount());
        return trackRepository.save(track);
    }

    @Override
    public List<Track> getAllTracks() {
        return trackRepository.findAll();
    }

    @Override
    public void deleteTrack(Long trackId) {
        trackRepository.deleteById(trackId);
    }

    @Override
    public List<Track> sortedTracks() {
        return trackRepository.findAll(Sort.by(Sort.Direction.DESC, "playCount"));
    }
}
```

- **Methods**:
  - `createTrack`: Converts `TrackRequest` to `Track` and saves it to the database.
  - `getAllTracks`: Fetches all tracks using JPA.
  - `deleteTrack`: Deletes a track by its ID.
  - `sortedTracks`: Fetches all tracks sorted by play count in descending order.

---

### `TrackRepository`

```java
package com.music.track.repository;

import com.music.track.model.Track;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface TrackRepository extends JpaRepository<Track, Long> {
}
```

- Extends `JpaRepository` to inherit basic CRUD operations.

---

## Recommendations for Improvement

1. **Error Handling**:
   - Use custom exceptions (e.g., `TrackNotFoundException`) for better error messages.
   - Add `@ControllerAdvice` for global exception handling.

2. **Input Validation**:
   - Use `@Valid` and `@NotNull` annotations in `TrackRequest` to validate incoming data.

3. **Testing**:
   - Add unit tests for `TrackService` and `TrackController`.
   - Use `MockMvc` for controller testing and Mockito for service layer testing.

4. **Pagination**:
   - Implement pagination using `Pageable` in the repository layer.

5. **Deployment Readiness**:
   - Create a `Dockerfile` and `application.properties` for production configurations.

---

## Study Areas
1. **Spring Boot Best Practices**: Error handling, DTO usage, and input validation.
2. **Testing**: Unit and integration testing with JUnit and Mockito.
3. **Deployment**: Familiarity with Docker, Kubernetes, or AWS.
4. **Advanced JPA**: Native queries, projections, and pagination.
5. **Spring Security**: Basics of authentication and authorization.
